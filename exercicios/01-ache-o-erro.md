# Exercício 1: Ache o erro

Quatro diagramas, um erro de projeto em cada. Para cada diagrama, abra pelo link ou importe o `.json` e:

1. Encontre o erro.
2. Identifique o que quebra e qual das [três regras](../pecas.md#três-regras) ele contraria.
3. Corrija o diagrama e rode de novo.

Nem todo erro aparece nos números. O simulador mostra carga e queda, não lógica. Quando o diagrama roda a 100% de sucesso, pause uma caixa de cada vez e leia os nomes.

## Diagrama A: Catálogo com cache

Arquivo: [`01-ache-o-erro-A.json`](01-ache-o-erro-A.json) · [abrir no simulador](https://app.stackmonkey.dev/#d=z.tZPBbtswDIZfxeCpA4zCUeqg9W3ruZfsOPTASJwjzBY9We4aBHmYPctebJKt2DKCArvsYoD8CfEj-fsMb2R7zQaqTQ5o5VE7km6wBNUZDCvqofp2Bq2gAmwGwz3k4E6d10E2mozzscE2xJ9HPbs7DL3ENjOcSXR_fjdc8ydfNZW_-Dd9LXdkfM6i89GmKIocWv2-J1RjmMM7VP57gmpbFJc8EkiUR0oAYhj7P4cwU5SNAAGUTK1NkCwpHRI92Tct6aUfX_fD7tFp9sH9Uw6_2P7w27jy_BxooGfsUGrnOcpItRW3XM1hgWoY1Rds0EiyC1vMoOKQxKZmq92xDWg8GLXngw77mBHKIrbbPdy2w67bLA3jTEuvryHhG2WhyA7G6THdndyRQxPJNpz1MZ1YPK6Xc1-UNxu43uVpNyGJbZkiiX9BEv8XabtbkFRyFIUOD9hTehAjObtbOXS2S8e9qwPRiqCcOUXCuXu4oRKRyo-wHO_Vv6_q-D99t9yu_iie7Rz4ozzbnSePJdLkOL6a4SNFpMrVNjwtZ62IVHm9XP4C)

Buscas no catálogo com balanceador, dois servidores, banco e um cache. 1.000 buscas por segundo.

Para testar: pause o cache.

## Diagrama B: Inscrições com dois servidores

Arquivo: [`01-ache-o-erro-B.json`](01-ache-o-erro-B.json) · [abrir no simulador](https://app.stackmonkey.dev/#d=z.zZMxbhsxEEWvQrCygS20lCUk2zmu3SRFisDFiJysCHM5a5KrWBAE5BC5QQofwMgJdJOcJEPtyruCEcClGwIzn5j_SH7u5AZDtORlVRYSgl7bhDp1AWW1k54MRll920lrZCXBdZ6iLGTatqxL7Sz6xLWHJtfXR11ceNhgDYbCJWv9pluexDuoRc-9AImrxayQjX38GmyuyhmXj7LidSur-Wy2LwZbtxotHYH5BA68xjAaD51syU1wNfHMdcNKoM6bz7Sy2fYHhXs-bXYezObqtR20bTkaRgwbq3H0-pIbbCRK8ffnL1F3EAwIEBFjPPwmceGotv5SeBANNofnYEE40uDyuTuf7HFKu01rykyaQr7ickL3oTi53nKlCvnQYYc30IK2aTve1PKqh1fzxRRevQVevRf4-XKEN5OHNpBgBRGnj-w1E1ofdbCHp8MfjDlf6Jn4SEUx1ZnnjGDxQqkmlMurV2BqAPu4HCNxx-NNPXyB74Gas09AfTYz_qD1SaVTiP6nqKlyihv1F3CuqKlyt9__Aw)

O sistema do Exercício 3 depois de "resolvido". 50 inscrições por segundo.

Para testar: pause o Servidor 2. O que o simulador mostra? O que ele não consegue mostrar?

## Diagrama C: "Sistema sem ponto único de falha"

Arquivo: [`01-ache-o-erro-C.json`](01-ache-o-erro-C.json) · [abrir no simulador](https://app.stackmonkey.dev/#d=z.vZTBjtMwEIZfxfJlQVSrJN1WkBtw3ks5cEB7mNrT1iLxGNtZtqoq8RC8AOLEgRPiwjVvsk_COEnblNUiTlyijH9n_GXmH-_kLfpgyMoyn0jwamMiqth4lOVOWtIYZPluJ42WpYSqsRTkRMatY12qyqCNHFuoU_yy08WTGqJvv6umgqcs9ruuORVvIYeW1zxEjvIsm8ja3C0QOP2sD956k7QU3cmSn1tZTrNsPxkgquUJoCLQr6ACq9CfMIYV0OTF_afP4gJDRBEMP2sQtv1Kgt-EIxtJtL-sUSQ0ihVUG7jgNFCtiSE2Nefy1Fi9oKVJ1B_Jv-dqJbisx5sWDwHBufyEGNDfGoUnujdpIaHlHVxgEuZj2FSWxkbT7XLbuKF0piKfWvB8dHpecDTkveY4u8xmE_mhwQZfgwNl4naoLRPOr3rCYjobExb_Qlj8V8Lp_EQYMIx9BmpzxhdC-xPZaJ0gFNUOfDTcPk3JcWjXxqadHrVJecYoE8kWX0A0xAiX2Zi66-oflLOB8sX8YR31yIoaIiwh4NiGlo3lvKnbL96QeCZ8-81VRkFyGwaHHvgX2h_OwBm1oxDXqaZn4LNjpYsR8_zqAXGRiD12J4VurIcgdmMuIWyt4twrMBXx9Kfk-dHRh_9M3bhhKL0eboCVp_rsDqB-GFM1Bq0fTTrMwGNKMVYO00KHrp9rxV-043d6-dhXSbnZ738D)

Matrícula da universidade. O autor garante que nada existe uma vez só.

Para testar: pause uma caixa de cada vez e veja o Success rate.

## Diagrama D: Cardápio da cantina, versão completa

Arquivo: [`01-ache-o-erro-D.json`](01-ache-o-erro-D.json) · [abrir no simulador](https://app.stackmonkey.dev/#d=z.tZZNThsxFICvYnkFaoTmJ6QwXVGqVpVKheiiiwpVb8YmcZmxB9sDBITUQ_QCLLvuEXKTnqS2ZxJ7mITSSmyi2O_F73v_ucWXVComOM7iEQZZzJimhW4kxdkt5oJQhbMvt5gRnGEoGy4UHmE9r40cFyWjXJszh8qeD5wc_f7-A9WL-ynjoNBWEkUICqqUkdRCIsLgFYKLhqHYnRWdNpyIERobxUt6QxWqgKlt82z7_pGBMI-LmnJzJ0FTx1qx6xMKBiuOohG-xpn5nOMsnezejXq8X1k1fRIzq2BKuUJEoAIkWdzXTPwnRZJ6ioLwwDwUM-qtH775aA6Um1jRlWohpI26eelKyHOTHpztRpF9X1F5yQp61EmBafs13h1hk7YT0IY3i3b2zfmioQ09hBoKpuft70dY5N9Mbo9yR54DJ1eM6NlRXivPnyZDD6Bm3gNzeGe8v4J5EMTj98hf9qj70DvRkC12WjaiH1jFtHMtAAkTWuaeoxRAXkMJvKDSk3Q3QIS9hHIqJNOzykikMGV2InLGh4jG3GTcmYuiwO869gY7P7ytT_bCGEJWSTZcM3ddz_VMBHmMA3N7vXAk62LhaPYnLU38skeTPIUmeTaaB7FJn0KTPhvNOPU0y7ba0GXdcdVnkhJm51i_n3o9FIC1BTrsKEcTJ3vDhiFBnRLQkIOiYY3yQqBasmpxL5lAL5Bc_KxLVgDaUotfqKTMDGDYDolrofTUhqwHvbsKZBLwTsYD2sS1GHVG2rB3B-1GPwY154XNlOmcY0nPqKSmi1xiC8EJSNvX1ttJNPT2jJXg_XWWvbNvW6EPPeQ509UFXoserxlzQ1cehj4cEe1Tj1Tm56XC5qoMidKkx5MOgaJ-ZEIYM3AfK4QDWcENtQeuBSIU2QGt3YZdxUulgzpdsx-6oKiZ2VttK9lMesbx2MmvzDAMQ7n317XgxuJ6h_cTPxVODTGZdv8VzqSoBttXdOvNxqZT6Baj6AIVSPwfDdGtn1DYbiPR7oNA0G4HsRzcmyTJRknaN9MOfxHMkzMoyxyKc5xp2dC-cvIvyumTlVeTTbRzZQNh14O1FDnkrGzztBMH2ssuFWGTmOZWTeVKaBzo-iZaJef07u4P)

200 acessos por dia, simulados aqui como 1 por segundo, 400 vezes mais. Um colega achou o Exercício 2 "simples demais" e melhorou.

Para testar: olhe quanto cada caixa está ocupada.
