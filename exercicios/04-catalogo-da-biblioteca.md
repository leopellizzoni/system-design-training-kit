# Exercício 4: Catálogo da biblioteca

Arquivo: [`04-catalogo-da-biblioteca.json`](04-catalogo-da-biblioteca.json) · [abrir no simulador](https://app.stackmonkey.dev/#d=z.tZNBbtswEEWvQnCVAEIgybHRatd0nU26yCLIYkSyMlGKowwpN4ZhoIfoBXKW3CQnCSmzFg0jQDbdCJj50vxH_eGObxQ5jZY3VcGBxFp7JfxIijc7blEqx5uHHdeSNxzMaNHxgvvtEHQujFbWh9pCH-tvk84u2tEJ6JlFJsC_vhjs8JK9_fnLqquyLNkkOzYgMae60UoMIw6zboNhGISDsqFH4ENVhW8K3uvnOwVyKgv-zJvw3PKmXiz3RcJrdfsx241ujcZwttcX0sAuBEhwnoAZvSF0iY-lNn6ebkK7J51QM7bFamYzGZpBkDdgwApFGeChAxJjE0yHYea6DwphoLjDVkfX30i_QmK8WZbJbFEnu7I82sEwVLOhU7TRQs1eP2IjGLH4Eo3W66k9bP0ao4lAirl_yeyqOlRp0G2oy6tyWfCnUY3qOwwgtN_O519dn6cTkOrPINX_FykPRWahSPDQglN5IFZgXJSTFXbYkhJApDqYVkPZTtsJFZ3vIuQJ1PKIXmfoq-sz0DqBfl3NcT6G8bJLV_AnYX9yCfGwV_E4SUs34Fwwx_60GR8pda782yE8_KlTpc6Vx_3-HQ)

**Cenário.** O catálogo da biblioteca recebe milhares de buscas por dia e quase nenhuma alteração. Os 50 livros mais buscados respondem pela maior parte do tráfego. No simulador: 1.000 buscas por segundo e 1 cadastro por segundo.

**Sintoma.** As buscas levam vários segundos, e o banco está sobrecarregado mesmo com o balanceador e dois servidores.

**O que você recebe.** Alunos e Bibliotecária → Balanceador → Servidor 1 e Servidor 2 → Banco (catálogo). Deixe rodar e veja o banco a 100%, a fila dele e o Success rate.

## Tarefa

1. Reduza a latência das buscas sem trocar o banco. Caixas esperadas: um **Cache** consultado pelos servidores, entre eles e o banco, e uma réplica de leitura, que no StackMonkey é o campo Replicas da própria caixa do banco (Replication async, Read preference secondary).
2. Ligue de modo que a busca vá primeiro ao cache e, se faltar, ao banco: Servidor → Cache → Banco, com **fallback** marcado na ligação Servidor → Cache. Hit ratio do cache: 90%.
3. No nome das caixas novas, ponha o sintoma que as justifica.

## Previsão (antes de mexer)

Se o cache reiniciar vazio às 14h, o que acontece com o banco nos minutos seguintes?

## Falhas para testar

1. Pause o cache. Por onde passam as 1.000 buscas por segundo? O banco aguenta?
2. A bibliotecária cadastra um livro às 10h00:00,0 e o busca às 10h00:00,4: "livro não encontrado". Por quê? Como contornar? Olhe o "lag" que a caixa do banco mostra.

## Perguntas

1. Com 1.000 buscas por segundo e 90% de acerto no cache, quantas chegam ao banco? E se o cache estiver vazio? Confira nas setas.
2. Onde o cache não pode ficar? Por quê? Dica: quem consulta o cache?

Pronto quando: cache e réplica justificados pelo sintoma; a conta dos 90% feita; você sabe explicar as duas falhas.
