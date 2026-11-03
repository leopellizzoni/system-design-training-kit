# Exercícios

Cada exercício tem um enunciado em `.md` e, para cada diagrama, um `.json` e um link que abre o diagrama no simulador.

| Ordem | Enunciado | Diagrama de partida |
|---|---|---|
| 1 | [Ache o erro](01-ache-o-erro.md) | [01-ache-o-erro-A.json](01-ache-o-erro-A.json)<br>[01-ache-o-erro-B.json](01-ache-o-erro-B.json)<br>[01-ache-o-erro-C.json](01-ache-o-erro-C.json)<br>[01-ache-o-erro-D.json](01-ache-o-erro-D.json) |
| 2 | [Cardápio da cantina](02-cardapio-da-cantina.md) | [02-cardapio-da-cantina.json](02-cardapio-da-cantina.json) |
| 3 | [Inscrições da Semana Acadêmica](03-inscricoes-da-semana-academica.md) | [03-inscricoes-da-semana-academica.json](03-inscricoes-da-semana-academica.json) |
| 4 | [Catálogo da biblioteca](04-catalogo-da-biblioteca.md) | [04-catalogo-da-biblioteca.json](04-catalogo-da-biblioteca.json) |
| extra | [Certificados do evento](extra-certificados-do-evento.md) | [extra-certificados-do-evento.json](extra-certificados-do-evento.json) |

## Como fazer cada um

1. Abra o diagrama pelo link do enunciado, ou importe o `.json`: ícone de download no canto superior direito, **Import a diagram file**.
2. Leia o cenário e o sintoma. Deixe a simulação rodar alguns segundos e olhe os cartões embaixo do quadro: Offered, Goodput, Success rate, P95 latency, Busiest e Estimated limit.
3. Faça a tarefa. Para acrescentar uma peça, arraste do painel **Components**; clique na caixa para editar o nome e os números; arraste da bolinha à direita de uma caixa até outra para ligar. No nome de cada peça nova, ponha o sintoma que a justifica.
4. Para testar uma falha, clique na caixa e, em Running, aperte **Pause**. Para um pico, use **Traffic spike** ou 2x, 5x, 10x. Na primeira mudança o simulador pede um nome para guardar o desenho no navegador; pode cancelar.
