# Exercícios de system design no StackMonkey

Exercícios de arquitetura de sistemas web no [StackMonkey](https://app.stackmonkey.dev), um simulador que manda requisições pelo diagrama e mostra fila, latência, taxa de sucesso e qual peça quebra primeiro.

Em cada exercício você recebe um diagrama de partida, um cenário e um sintoma. A tarefa é fazer só a mudança que o sintoma pede, pausar uma caixa para ver o que acontece quando ela cai e apontar o ponto único de falha (SPOF). Às vezes a mudança é tirar uma peça, não acrescentar.

## Antes de começar

1. Leia [as peças](pecas.md): três regras e, para cada peça, o que é, quando usar, o que custa e como aparece no simulador.
2. Abra o simulador em [app.stackmonkey.dev](https://app.stackmonkey.dev). Não precisa de conta. A interface é em inglês.
3. Cada diagrama tem um `.json` e um link. O link abre o diagrama direto no simulador. O `.json` você importa pelo ícone de download no canto superior direito, em **Import a diagram file**.

## Respostas

As respostas estão em [`respostas/`](respostas).

## O caminho

| # | Exercício | O que você pratica |
|---|---|---|
| 1 | [Ache o erro](exercicios/01-ache-o-erro.md) | Quatro diagramas prontos com um erro em cada. |
| 2 | [Cardápio da cantina](exercicios/02-cardapio-da-cantina.md) | O mínimo: cliente, servidor, banco. Caminho de leitura e de escrita. |
| 3 | [Inscrições da Semana Acadêmica](exercicios/03-inscricoes-da-semana-academica.md) | Balanceador e servidores sem estado. Onde fica a sessão. |
| 4 | [Catálogo da biblioteca](exercicios/04-catalogo-da-biblioteca.md) | Cache e réplica de leitura. O que acontece quando o cache some. |
| extra | [Certificados do evento](exercicios/extra-certificados-do-evento.md) | Fila, worker e armazenamento de objetos. Síncrono e assíncrono. |
