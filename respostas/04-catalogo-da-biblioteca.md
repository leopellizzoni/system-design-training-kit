# Resposta 4: Catálogo da biblioteca

Diagrama esperado: [`04-catalogo-da-biblioteca-gabarito.json`](04-catalogo-da-biblioteca-gabarito.json) · [abrir no simulador](https://app.stackmonkey.dev/#d=z.zVSxbhsxDP0VQVOCGsH5EhvNdWoyByjSoUORgadTbCE68Srp0hiBgU4Fuhb9gYxFxwzd6z_xl5SSlbNcI0DGLjZI2uR7j4-657fSOoWGV-MRByvmykvheyt5dc8NNtLx6uM9Vw2vOOjeoOMj7hcd1bnQShpPsYE2xG9jnR3UvRPQMoNMgF89aJzhIVt_-cHGR0VRsFh2rEPLnJz1pkFqsel1QQOpEXbSUM6Cp2hM_xnxVt1dSmhiOOJ3vKLPBa_K48lylODVqn4e25mqtULitnqwCtiBgAact8C0urXoEj6W0vhydBHaB6sS1Azb8XSLTWfQNEJzBhqMkDYDuMlAgyEJeobUc95SxSKhuMRahamf0d7Qxng1KdKw4zKNK4phHHTdeDvQSXurhNzOeh8SNIgIB9pxIRVxF3PJ1l-_M7v62Wkl4M2gR8U6q9qgXZDD9sar2Klb-DkGXAJtsMrrDOG4pCjNvqC4OComI_6pl708hw6E8outZNOT_YUSi_IlLMr_jkW--ggoM2YKE4nzCLeRT1cRmDhlPLbEheJWupa-JsPZWNlJr4gP9ZBmpkzoYmWjQiLHOeJ0y5fgiSuBPs0pRev8Q2GSKJxO9-3UZO5twEMNTubONQK3yrJXg_LswK0emZaKHhQ43OWWsmz97ReTTpDZgf35zYBu0mEQRKweOwU5yw6dn4UF7RCdDGsrM47Tkz2GZWBI8gVkLl5uCnx8_zi4hRHBF3SE76y8llbSQUbnCTQN2AWPCpEjthJdEb5mll7Ja4vtzjuJm9MPQqZaeqT2C3rIx-N9rlLmlaczx8xX16B1DeKGV972cvfH5Yt_PJgWN-tfXi2XfwE)

Servidores → Cache (com fallback) → Banco com uma réplica de leitura (Replicas = 1, Replication async, Read preference secondary). A busca vai ao cache e, se faltar, ao banco, que responde leituras pela réplica; o cadastro vai ao primário.

No simulador: o diagrama de partida mostra o banco a 100%, com fila de quase 200 consultas, latência acima de 1 s e Success rate perto de 35%. Com o cache, as 1.000 buscas viram cerca de 100 no banco, que fica em torno de 30%, e o Success rate vai a 100%.

**Pergunta 1.** Com 90% de acerto, 10% das 1.000 chegam ao banco: 100 por segundo. Cache vazio: 1.000 por segundo, dez vezes mais. Com 99% de acerto o salto seria de cem vezes: quanto melhor o cache, maior o salto quando ele some. As setas do simulador mostram a conta: 1.000 entrando no cache, cerca de 100 saindo para o banco.

**Previsão e falha 1.** O banco recebe toda a carga de uma vez; se não está dimensionado para isso, as buscas param. No simulador: pause o cache e, como a ligação tem fallback, as 1.000 buscas caem no banco, que volta a 100% e a falhar. O que se faz: aquecer o cache antes de receber tráfego; quando a mesma chave falta, só uma requisição vai ao banco e as outras esperam; limitar o que chega ao banco e devolver erro rápido; ter mais de uma cópia do cache. O banco atrás de um cache precisa aguentar a carga sem o cache, ou é preciso um plano para o cache vazio.

**Falha 2.** Atraso de replicação: a escrita foi para o primário e a busca foi para a réplica, que ainda não tinha a cópia. Outra causa possível: o cache guardou a lista antiga (TTL). Contornos: ler do primário logo depois de gravar; avisar "pode levar alguns segundos"; TTL adequado. O atraso não é defeito do banco; é o preço de ler de uma cópia. A caixa do banco mostra esse atraso como "lag"; com 1 cadastro por segundo ele fica perto de zero, e é por isso que o simulador não reproduz o livro que some: o atraso real vem da cópia entre máquinas.

**Pergunta 2.** O cache não fica entre o cliente e o balanceador: quem consulta o cache é o servidor de aplicação, que sabe o que está pedindo e o que guardar.

**Erros comuns.**

- Cache antes do balanceador (é o Diagrama A do Exercício 1).
- Achar que a réplica resolve a escrita.
- Tratar o atraso de replicação como defeito do banco.
- Ligar a bibliotecária direto ao primário, pulando balanceador e servidor.
