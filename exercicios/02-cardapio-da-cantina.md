# Exercício 2: Cardápio da cantina

Arquivo: [`02-cardapio-da-cantina.json`](02-cardapio-da-cantina.json) · [abrir no simulador](https://app.stackmonkey.dev/#d=z.jdE7bsMwDAbgqwicEsCDndegrc2cpUuHIgMjES7RWHIk2c0DBnqIXqBn6U16ktJOAKNDiy4CqJ-gPogXaClE9g50kQEG88yJTGoCgb6A85Yi6KcLsAUNuG-cj5BBOtWSg9kzuSS1w6qv74ZcTRy2VKL1Yaq-3t7VLM8VGopRstoHZRnVBA8Nq2KoI5WNsz5TC2ls6UxRVchxKoOvL2yEIeN9TU7uAiYatBUfHwgFVuR5BkfQcp5Az-bLLruJDbrEDn8nr68N4kkN7vmMyiuDwX5-1Oyv_KI3_QX_D_Qx8FD8kM5Xo9TuRqTFhDuMNDLv0RmvJqNMInIluz6sfUxloH4xkULLhjaytKWgfH-rZxm8-vAiewa9WmRwaKihNdZoOPX_dTOtFjdVnndbGW_LYffbrvsG)

**Cenário.** A cantina quer publicar o cardápio do dia no site. São cerca de 200 acessos por dia, e o cardápio muda uma vez por dia, quando a cantina o atualiza. No simulador a carga é de 1 acesso por segundo, 400 vezes a real, para os números aparecerem.

**O que você recebe.** Três caixas soltas: "Alunos (navegador)", "Cantina (atualiza o cardápio)" e "Banco (cardápio)". Falta a caixa que executa o sistema, e faltam as ligações. Enquanto nada está ligado, o simulador fica em "Not running" e os clientes não mandam nada.

## Tarefa

1. Acrescente a caixa que falta (**Service**) com o nome "Servidor de aplicação (cardápio)".
2. Ligue as caixas de modo que o aluno consiga ler o cardápio e a cantina consiga atualizá-lo.
3. Siga no diagrama o caminho de leitura e o caminho de escrita, caixa por caixa (Aluno → … → …). Os números nas setas mostram quantas chamadas passam por cada uma.

## Previsão (antes de mexer)

Se o servidor de aplicação parar por 1 hora, o que o aluno vê? E a cantina?

Para testar: pause o servidor e olhe o Success rate dos dois clientes.

## Perguntas

1. Qual é o ponto único de falha deste sistema? Há mais de um?
2. Para 200 acessos por dia, vale a pena eliminar esse ponto único de falha? Compare o custo de ficar 1 hora fora com o custo de ter duas cópias.

Pronto quando: o diagrama tem no máximo 4 caixas, você consegue seguir os dois caminhos e justifica a pergunta 2 pelo custo.
