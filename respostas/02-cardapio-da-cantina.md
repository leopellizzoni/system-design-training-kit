# Resposta 2: Cardápio da cantina

Diagrama esperado: [`02-cardapio-da-cantina-gabarito.json`](02-cardapio-da-cantina-gabarito.json) · [abrir no simulador](https://app.stackmonkey.dev/#d=z.jdK9TsMwEADgV7FuKlKGNC0VygZIbAgEAwNiOOwjWLQ-YzuFUlXiIXgBxMCD9E14Euw0kJY_sUQ6n-P7zr45TMl5zQbKfgbo5LUOJEPtCMo5GFbkoTyfg1ZQAo5rwx4yCDMb8yDHmkyIscFJinebvOgZnFKFit2WeHt8EkWeC5TkfcxZdkJpFD28rbXoN7GnqjaKMzGMG6f0QF5MUPutePCqwmFkxOPZkolrDgM12om-PyGMsH6eZ3APZfzOoCwG24usFUs0QRv8nby_2hA9ocaxfkDBQqJTy2erecXvJ9Nf8P9Az5xugg3pYNRJ0dpO6clNtaSOeZoWVCKQQDvWEpevyxcWvS_W0-Ojg1S5jl01P9pZuOaEkezSU0bNHbub-OZQ7mQfhQ5jVGRwW1NN-2hR6jDrsIOi5eb5J1dddlqFAS_Rr3H30MhfcWQqbRob-1Al1YZj-9NarFlHw2-8ouWNhh3vIh6vqnZmrxxPNqaW23tOXbTJtQH5Ids-Cq8aXlwsFu8)

Alunos → Servidor de aplicação → Banco; Cantina → Servidor de aplicação → Banco. Quatro caixas, três ligações.

**Caminhos.** Leitura: Aluno → Servidor → Banco → Servidor → Aluno. Escrita: Cantina → Servidor → Banco, e de volta com a confirmação. As setas mostram 1 chamada por segundo de cada cliente passando pelo servidor e chegando ao banco.

**Previsão.** Com o servidor fora, ninguém lê nem atualiza: o navegador mostra erro ou "tempo esgotado". O banco continuar vivo não ajuda, porque nada o consulta. No simulador: pause o servidor e o Success rate dos dois clientes vai a 0%, com o banco ocioso.

**Pergunta 1.** Servidor e banco são pontos únicos de falha: cada um existe uma vez só.

**Pergunta 2.** Não vale. Se ficar 1 hora fora, alguns alunos olham o mural; duplicar custa mais do que o dano. A justificativa é o custo. "Tudo redundante" não é resposta de engenharia. No simulador, mesmo com 400 vezes a carga real, o servidor fica em 2% e o banco em 0%.

**Erros comuns.**

- Acrescentar cache, CDN ou balanceador: desproporcional para 200 acessos por dia (regra 3).
- Ligar a cantina direto ao banco, pulando o servidor: onde fica a regra de negócio e o login?
- Ligar o aluno direto ao banco.
