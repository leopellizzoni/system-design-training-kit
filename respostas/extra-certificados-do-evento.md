# Resposta extra: Certificados do evento

Diagrama esperado: [`extra-certificados-do-evento-gabarito.json`](extra-certificados-do-evento-gabarito.json) · [abrir no simulador](https://app.stackmonkey.dev/#d=z.zVXNbtswDH4VQZemgDfYzg8679R16C1t0R56GHqQbcbRFouupCTNigJ7gd36DnuE3dc32ZOMsp1aaZJux12CUJTJ7yM_Uvd8AdpIVDyJAi50NpUWMjvXwJN7rjAHw5NP91zmPOGV0JYH3K4q8vJsJkE5W4nS2RfklZmshLJgWI_cmSgZlOwASmmlZhnQhQmd5nhwyH5_e2RDVqFmBoq5ypEiNSHHlJXiYQWKzrSwZA0DXsq7ay2dEYVhwO94Qr8rnvTD8CFoEc7SDt8MRf5BzISivB3K9oQwuEMxK5BiTkvyaCQUl5hKl3WJ-gsVhhKHbbJ-vJ1OVFXUJTSgFzKDLteVO6BELKrZXj19Pzu5PD87T1ihxUIwZBXkdCFgoCZyBlILBkyDqVDlwJRgU9TC1WCurKxDVis7RQcwQ-1aE3lQj4I1hDFZccBv5zCHE1GJTNpVV7bRoGES94c-k_hfmMT_JZP-qGNC6UXHpP6w43FKTkaAPCmamtHx1TMn9usnM1JZLEXCSJEUghERIEr9kBkKBqqQyoXTIk2lLW87FrHHIqql4xEJt4jE4Vpe70bbTck9NefCilQY8JWsMmS9pvDmPVW7kMYSygOPHTujjixkPXI-8gqNLRziDYDDnTxGg124N1D7DWi-e0VM1_WFl2UnKbkSI7v4eEraqVGTBW9KIWeuJzKHskJLCwJou0whE6xZNXl7Wx--KjASVaUxA2OcNfAI9uONKgy2-9SwjeKj7R2A6efXunSsS_EVnEGKclDpPlhSXY94mo2emD5_KZcWeuiBbSVjpkLnDStN26yDOhjU_qVblV7DjupTlzyz47Qet1SofClzOx2nlWmHaTfvUbgtTnB9-dvKePpRc26b2KO_E9oDm30qkO8kGm1Pz1JIO25rsAvrhLJc1u9F-DaMN7E7id5QtfOifdImGkvvUcPm-XD8Wk_zmOB6z-_zxL5n_SJgM76bnniv5_mbenvt-eqlb73p0B-6DJWZl80W9e52Q4mtZvc6m8budTvsNw8PfwA)

Síncrono: Participante → Balanceador → Servidor → Banco (grava o pedido) e → Fila (publica) → resposta na hora. Assíncrono: Fila → Worker → Armazenamento de objetos (PDF), → Serviço de e-mail e → Banco ("certificado N enviado").

No simulador: no diagrama de partida os servidores geram o PDF e esperam o e-mail dentro do clique; ficam a 100% com 1 a 2 pedidos por segundo de capacidade para 5 que chegam, a latência sobe a cada segundo e os pedidos passam a falhar quando a fila deles enche. No corrigido, o clique responde em milissegundos e o Success rate vai a 100%; o trabalho pesado aparece no worker e no serviço de e-mail, fora do caminho do participante.

**Previsão.** 0,1 s depois: "Pedido recebido; o certificado chega por e-mail." 2 min depois: o e-mail chegou, ou está na fila, se houver 5.000 na frente. O site não fica lento para os outros, porque o trabalho pesado saiu do caminho do clique.

**Falha 1.** Os pedidos não se perdem: ficam na fila e são processados quando os workers voltam. A fila existe para isso. O que cresce é o tempo até o e-mail chegar. No simulador: pause o worker e a fila cresce enquanto o Success rate dos participantes continua em 100%; volte a rodar o worker e a fila esvazia.

**Falha 2.** Sim, recebe dois, se a tarefa não for idempotente: a fila entrega "pelo menos uma vez" e reentrega o que não foi confirmado. Contramedida: antes de enviar, checar no banco se "certificado N" já foi enviado; registrar o envio; só então confirmar à fila. Idempotente: executar duas vezes dá o mesmo resultado que uma. É a mesma ideia de uma migration bem escrita. O simulador não mostra isso: é lógica.

**Pergunta 1.** Banco não é lugar para arquivo grande: 5.000 PDFs incham o banco, tornam o backup lento e disputam com as consultas; o armazenamento de objetos guarda por URL.

**Pergunta 2.** A fila cresce sem parar. Sabe-se medindo o tamanho da fila (métrica) e criando mais workers quando ela cresce. No simulador: com 2x de tráfego a fila do worker começa a acumular; o número na caixa da fila é a métrica.

**Erros comuns.**

- Guardar o PDF no banco.
- Worker sem checagem antes de enviar (duplicidade).
- Servidor de aplicação ainda ligado ao serviço de e-mail (continua síncrono).
- Não medir o tamanho da fila.
