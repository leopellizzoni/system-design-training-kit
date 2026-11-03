# Exercício 3: Inscrições da Semana Acadêmica

Arquivo: [`03-inscricoes-da-semana-academica.json`](03-inscricoes-da-semana-academica.json) · [abrir no simulador](https://app.stackmonkey.dev/#d=z.ZdFLasMwEAbgq4iBQgJeOM6Dol2bdaBQShclC0WeOKKJxpXkNCYYeojeoSfoptvcpCfp2E7qhGwEvwaNPo32sEXnDVmQgwiU0ysTUIfCIcg9WErRg3zZg0lBgloXljxEEMqc66DXBm3gbNWmzndNXfSs2mKmUnJ98fvxKcaxMNZrZw5fh2_0IicnPGaFTYnPtk1mfBN3oBwt7zkVOI3jCDZm9-xMnQYxxx1IXkuQwziuohMrzzuTR7c1GjvUY73BFnH4sUZTI5o-PAnudxOJpVkroR16jS3HFTaY5mBehhXVGk2ungLP553cK48L5G10umjGKYngrcACpypX2oSyww6Ta2666LSpCmqh_Bn3XllG9s4H1ucq2szYRkU-ZLXnQjD-VyZnysnoCpYcYZNRB5tz-zQ7fvTS0ebiq-k44dp_KrYDp_Yx1byq_gA)

**Cenário.** As vagas abrem às 10h. 3.000 alunos tentam se inscrever nos primeiros 5 minutos: cerca de 50 requisições por segundo. O servidor único conclui 40 por segundo.

**Sintoma.** Às 10h01 o servidor está em 100% de CPU, a fila de requisições cresce e o site cai.

**O que você recebe.** Alunos → Servidor único (CPU 100%) → Banco. Deixe rodar e veja: o servidor a 100%, a fila, a latência e o Success rate.

## Tarefa

1. Faça o sistema aguentar o pico sem trocar o banco. Caixas esperadas: um **Load balancer** e dois ou mais **Service** iguais ao servidor único.
2. No nome de cada caixa nova, ponha o sintoma que a justifica. Exemplo: "Balanceador — sintoma: 1 servidor a 100% de CPU".
3. Configure o balanceador: Algorithm = Round robin.
4. Decida onde fica a sessão (o login) do aluno e represente isso no diagrama.

## Previsão (antes de mexer)

Com o balanceador e dois servidores, o que acontece se um servidor cair durante o pico? Quantas requisições dão erro? Lembre da prática com nginx e do cabeçalho X-Atendido-Por.

## Falha para testar

Pause o Servidor 2. Que caminho para? Que caminho continua? O que acontece com a fila do Servidor 1? Depois volte a rodar o servidor.

## Perguntas

1. Onde fica a sessão do aluno? Por que ela não pode ficar na memória de um servidor?
2. Qual é o novo ponto único de falha?
3. O banco virou o novo gargalo? Por quê?

Pronto quando: o sistema continua respondendo com um servidor fora; a sessão está fora do servidor, com o lugar nomeado no diagrama; você sabe dizer o novo ponto único de falha e o gargalo.
