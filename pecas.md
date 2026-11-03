# As peças de um sistema web no StackMonkey

Toda peça existe para resolver um problema e traz um custo ou um modo de falha novo. Antes de pôr uma peça no diagrama, diga qual sintoma ela resolve.

## Três regras

1. Comece com o mínimo (cliente, servidor, banco) e só acrescente uma peça quando um sintoma pedir.
2. Toda peça que existe uma vez só é um ponto único de falha. Pergunte sempre: "e se esta cair?"
3. Peça nova é custo e complexidade novos. Uma solução com 15 peças para 100 usuários está errada, mesmo que funcione.

## Como o StackMonkey simula

Uma ligação significa "chama". A requisição nasce no cliente e segue as setas; cada caixa tem uma capacidade e uma fila. Chegando mais do que cabe, a fila cresce, a latência sobe e as chamadas passam a falhar. Os cartões embaixo do quadro resumem: Offered (quanto chega), Goodput (quanto é respondido), Success rate, P95 latency, Busiest (a caixa mais ocupada) e Estimated limit (quanto o sistema aguenta e qual caixa estoura primeiro).

Para testar uma queda, clique na caixa e, em Running, aperte **Pause**. Para um pico, use **Traffic spike** ou 2x, 5x, 10x. Na primeira mudança o simulador pede um nome para guardar o desenho no navegador; pode cancelar. Os números de cada caixa (núcleos, workers, CPU por chamada) são estimativas: o que importa é a ordem de grandeza e a comparação entre dois desenhos.

## As peças dos exercícios

| Peça | Para quê | No StackMonkey |
|---|---|---|
| [Cliente](#cliente) | origem de toda requisição | Client |
| [Servidor de aplicação](#servidor-de-aplicação) | executa a regra de negócio | Service |
| [Banco de dados](#banco-de-dados) | guarda os dados de forma permanente | Database |
| [Balanceador de carga](#balanceador-de-carga) | um servidor não dá conta | Load balancer |
| [Cache](#cache) | a mesma leitura se repete milhares de vezes | Cache, entre o servidor e o banco |
| [Réplica de leitura](#réplica-de-leitura) | muito mais leitura do que escrita | campo Replicas da caixa Database |
| [Fila de mensagens](#fila-de-mensagens) | uma tarefa lenta trava a resposta | Queue |
| [Worker](#worker) | alguém precisa processar a fila | Worker |
| [Armazenamento de objetos](#armazenamento-de-objetos) | arquivos grandes | Object storage |
| [Serviço externo](#serviço-externo) | a função não é sua | External API |

Outras peças aparecem nos diagramas, mas nenhum exercício pede que você as acrescente: [CDN](#cdn), [API Gateway](#api-gateway) e [Monitoramento](#monitoramento).

## Cliente

**O que é.** O navegador (ou app) de quem usa o sistema. Origem de toda requisição.

**Quando usar.** Sempre. É o ponto de partida de cada caminho que você vai traçar.

**No StackMonkey.** Bloco **Client**. O campo principal é a taxa, em requisições por segundo. No nome, diga quem é o usuário: "Alunos (navegador)".

**Exemplo.** O cardápio da cantina tem dois clientes: os alunos, que leem, e a cantina, que atualiza. Dois rótulos, dois caminhos.

## Servidor de aplicação

**O que é.** O programa que executa a regra de negócio e responde ao cliente.

**Quando usar.** Sempre. Dois ou mais quando um só não dá conta, ou quando o sistema não pode parar com um servidor fora.

**O que custa.** Vários servidores só funcionam se forem sem estado: nada do usuário fica só na memória de um deles. O que está na memória do Servidor 1 não existe para o Servidor 2.

**No StackMonkey.** Bloco **Service**. Linguagem, núcleos, workers (chamadas ao mesmo tempo) e CPU por chamada definem quantas requisições por segundo ele aguenta; o cartão Estimated limit mostra o número. Dois servidores são duas caixas atrás do balanceador, para você poder pausar uma delas.

**Exemplo.** Um servidor conclui 40 requisições por segundo e chegam 50: a CPU vai a 100%, a fila de requisições cresce, o site cai. Quantos servidores para 50 req/s com 30% de folga? 50 ÷ (40 × 0,7) ≈ 1,8, ou seja, dois. Para continuar no ar com um deles fora, três.

## Banco de dados

**O que é.** Onde os dados ficam guardados de forma permanente.

**Quando usar.** Sempre que algo precisa sobreviver a um reinício.

**O que custa.** Normalmente é o primeiro gargalo e o ponto único de falha mais difícil de eliminar: a escrita não se divide entre cópias.

**No StackMonkey.** Bloco **Database**: pool de conexões, fila e CPU por consulta. A replicação é um campo da própria caixa, não uma segunda caixa.

**Exemplo.** Um banco que grava 30 registros por segundo continua gravando 30 com dois, três ou dez servidores na frente. Mais servidores resolvem CPU, não escrita.

## Balanceador de carga

**O que é.** Recebe todas as requisições e as distribui entre vários servidores. Para de mandar para o que caiu.

**Quando usar.** Um servidor chega perto de 100% de CPU, ou o sistema não pode cair com um servidor fora. É a peça que torna a escala horizontal possível.

**O que custa.** Sozinho, é um novo ponto único de falha: todas as requisições passam por ele.

**No StackMonkey.** Bloco **Load balancer**, com o algoritmo (round robin, least connections, random). Ligue o balanceador a cada servidor. O simulador não modela dois balanceadores com o mesmo endereço: é sempre uma caixa só.

**Exemplo.** Round-Robin com dois servidores: a requisição 1 vai ao Servidor 1, a 2 ao Servidor 2, a 3 ao Servidor 1, e assim por diante. Se o Servidor 2 cai, tudo passa a ir ao Servidor 1: o sistema continua, com metade da capacidade.

**Sticky Sessions.** Com o mesmo usuário indo sempre ao mesmo servidor, a sessão parece resolvida, mas se o servidor cai a sessão some junto. É atalho, não solução.

## Cache

**O que é.** Memória rápida que guarda respostas já calculadas, para não consultar o banco toda vez.

**Quando usar.** A mesma consulta se repete milhares de vezes e o banco está sobrecarregado de leitura. Também guarda a sessão compartilhada entre servidores.

**O que custa.** Se cai, toda a carga vai ao banco de uma vez. O dado pode ficar velho até o TTL vencer.

**No StackMonkey.** Bloco **Cache**, na cadeia de chamadas entre o servidor e o banco: Servidor → Cache → Banco. Hit ratio é a parte das leituras que o cache responde; o resto segue para o banco. A ligação Servidor → Cache tem a opção **fallback**: com ela, se o cache cair, as chamadas seguem direto para o banco; sem ela, falham. Para sessões, o cache é ligado de cada servidor, sem nada atrás.

**Exemplo.** 1.000 buscas por segundo com 90% de acerto: 100 chegam ao banco. Cache vazio: 1.000 chegam, dez vezes mais. Com 99% de acerto, o salto seria de cem vezes. Quanto melhor o cache, maior o baque quando ele some.

## Réplica de leitura

**O que é.** Cópia do banco que só atende consultas. Toda escrita continua indo para o primário, que copia para a réplica.

**Quando usar.** Muito mais leitura do que escrita: catálogo, feed, cardápio.

**O que custa.** A cópia atrasa alguns instantes. Quem grava e lê em seguida pode não ver o que gravou. Não resolve escrita.

**No StackMonkey.** Não é uma caixa: na caixa **Database**, o campo Replicas, com Replication async e Read preference secondary. A caixa passa a mostrar "+1 replica" e o atraso da cópia.

**Exemplo.** Feed de notícias: 10.000 leituras para cada publicação. Leituras na réplica, publicação no primário. Quem publica e recarrega a página meio segundo depois pode não ver o próprio post: a réplica ainda não recebeu a cópia.

## Fila de mensagens

**O que é.** Lista de tarefas pendentes que um serviço deixa para outro processar depois.

**Quando usar.** Uma tarefa lenta (gerar PDF, enviar e-mail) está travando a resposta ao usuário, ou para absorver picos.

**O que custa.** A fila entrega "pelo menos uma vez": a mesma tarefa pode chegar duas vezes. Se chegam mais tarefas do que os workers dão conta, a fila cresce sem parar. Meça o tamanho da fila.

**No StackMonkey.** Bloco **Queue**. O servidor publica na fila e responde na hora; a ligação Fila → Worker diz quantos consumidores leem dela.

**Exemplo.** Loja virtual: o pedido é confirmado na hora, e a nota fiscal, que leva 5 s para emitir, vai para a fila. Se o emissor de notas cair por 10 minutos, os pedidos não se perdem: ficam na fila esperando.

## Worker

**O que é.** Programa que fica pegando tarefas da fila e executando, em segundo plano.

**Quando usar.** Sempre que existe uma fila.

**O que custa.** A tarefa precisa poder rodar duas vezes sem estragar nada (idempotente), porque a fila pode entregar a mesma tarefa de novo.

**No StackMonkey.** Bloco **Worker**, ligado a partir da fila. Pause o worker e a fila cresce sem que o cliente perceba: é isso que separa o caminho síncrono do assíncrono.

**Exemplo.** Worker que envia e-mail: antes de enviar, checa no banco se o e-mail N já foi enviado; envia; registra; só então confirma à fila. Se cair entre enviar e confirmar, a fila reentrega, e a checagem evita o segundo e-mail.

## Armazenamento de objetos

**O que é.** Depósito de arquivos grandes (PDF, fotos, vídeos), acessados por endereço (URL).

**Quando usar.** O usuário envia ou recebe arquivos. Banco de dados não é lugar para arquivo grande: incha o banco, atrasa o backup e disputa com as consultas.

**No StackMonkey.** Bloco **Object storage**.

**Exemplo.** Foto de perfil: o arquivo fica no storage; o banco guarda só a URL.

## Serviço externo

**O que é.** Sistema de terceiros que o seu chama: envio de e-mail, pagamento.

**Quando usar.** Quando a função não é sua.

**O que custa.** Tem limite e latência próprios. Mais servidores seus não o fazem responder mais rápido. Chamado dentro da requisição, faz o usuário esperar por ele.

**No StackMonkey.** Bloco **External API**: latência fixa, limite de chamadas ao mesmo tempo e uma parte das chamadas falha.

**Exemplo.** O envio de um e-mail leva 2 s. Dentro do clique, o usuário espera 2 s a mais. Atrás de uma fila, o usuário espera 0,1 s e o e-mail chega depois.

## CDN

**O que é.** Rede de servidores espalhados que guarda cópias de arquivos estáticos (imagens, vídeo, JS) perto do usuário.

**Quando usar.** Muitos acessos a arquivos grandes e iguais para todos. O pedido nem chega ao seu servidor.

**No StackMonkey.** Bloco **CDN**: um cache na borda, com hit ratio, ligado ao storage ou ao servidor de origem.

**Exemplo.** O vídeo de abertura do evento, assistido por 5.000 pessoas, sai da CDN. Um cardápio em texto com 200 acessos por dia não tem o que distribuir.

## API Gateway

**O que é.** Porta de entrada única para vários serviços: autenticação, limite de requisições, roteamento.

**Quando usar.** Há mais de um serviço de back-end exposto e cada um repetiria login e controle de abuso.

**No StackMonkey.** Bloco **API gateway**: roteia por tipo de chamada e limita a taxa.

**Exemplo.** Matrícula, biblioteca e cantina atrás de um gateway: o login é checado uma vez, na entrada. Com um serviço só, o gateway é peça a mais.

## Monitoramento

**O que é.** Painel com métricas, registros e alertas.

**Quando usar.** Sempre, a partir de produção. Não fica no caminho da requisição: observa todas as peças.

**No StackMonkey.** Não existe como bloco. Os cartões e os gráficos do próprio simulador fazem esse papel.

**Exemplo.** Um alerta "fila com mais de 1.000 tarefas há 5 minutos" avisa que faltam workers antes de alguém reclamar.
