# Resposta 1: Ache o erro

## Diagrama A: Catálogo com cache

Diagrama corrigido: [`01-ache-o-erro-A-gabarito.json`](01-ache-o-erro-A-gabarito.json) · [abrir no simulador](https://app.stackmonkey.dev/#d=z.tZNLbtswEIavQnCVAkIhy5HRaNdmnY27LLIYkROZCMVRSSqNYRjoIXqBokfJTXqSkhJj0TUKZNMNgXlg5puZnwf-hNYpMrxZFRys2CmPwo8WeXPghiQ63nw5cCV5w0GPhhwvuN8PIc6FVmh8sA300f44xdlVOzoBPTPEBPiXn5o6ehey5vS7UDPk0oAm-Cz4YK3Ksix4r563CHIyC_7Mm_DuebMuy2ORCHS7dNcE8hNoMALtwpA8ICk6QXdkld_1IWJpNHJLrYp9v5F9DIPzpi5Ts3V12Q6GYbU0dGiflMCl1-foCI1YTLKj8WpyD3u_o9hEkI3r-5C1W1XBSoXugl2-L-uCfx1xxFsYQCi_X-bfXM9I1brOkaq3IFX_F2m9WZAEiB1mqkhmIrqNJpPIJlU49vv7DybIuFH7cCQ2oA6ScQkbXcHMy6_ZzWbBxFpoOmViNYtSRQXmvAUPot2CVxThb_LRpuP-NUqdRrnZXB5cZvqS4KEFh7m2jCB2dSbqE9lAzndxuWdw9WnlVca1ub6gqhJVuMaCdR_qyy59wQdL_dknpPlDRPgUm78HvSr3X5Eqj7xqnLLTPYDWLYhH3ng74nly9ebkky5o3uzx_nj8Aw)

O cache está entre o cliente e o balanceador. Quem consulta o cache é o servidor de aplicação, que sabe o que pedir e o que guardar; na frente do balanceador, o cache não sabe o que é cacheável nem quando invalidar, e vira mais um ponto único de falha no caminho de todas as requisições. Correção: cache ligado aos servidores, entre eles e o banco. Uma CDN na frente é outra coisa: só arquivos estáticos.

No simulador: com o cache ligado, os dois diagramas rodam a 100%. Pause o cache. No diagrama errado, tudo falha: o cache estava no caminho de todas as requisições. No corrigido, as chamadas seguem para o banco (a ligação tem fallback), e o banco, que recebia 100 buscas por segundo, passa a receber 1.000 e satura. É o cache vazio do exercício do catálogo.

## Diagrama B: Inscrições com dois servidores

Diagrama corrigido: [`01-ache-o-erro-B-gabarito.json`](01-ache-o-erro-B-gabarito.json) · [abrir no simulador](https://app.stackmonkey.dev/#d=z.vZPBjtMwEIZfxfJpK0WoSbcV5Aac97IcOKA9TO1pYpF4gu2UrapKPARvwIEn4An6JjwJ4zTduFst4sQl0szvzP_NeLyXW3TekJVlnklwqjYBVegdynIvLWn0svy0l0bLUkLTW_Iyk2HXsS5VY9AGji20MX476OLGwhYr0ORmrJ0O3XElPkEdWs45CBwt55lszeNHZ2KUzzl8lCV_d7JczOeHbLRt1pNlQ6DfQQNWoZuMx0y05CQ0FXHNumXFUW_1Pa1NtP1K7jN3G51Hs0VxbQddl0-GHt3WKJy8PsQEG4lc_P72XXhsBfrA1rGv3gYznOp2oaboqcjFEeaJ--vsXPWOoyKTX3rs8T10oEzYTZNY3Z7gisUyhSv-Ba74X3CL1QTn0afbAaq-QPP--At5PQZBKGo7cME0NePNBlwQscLxB4mNUSA25EBoEn5simuhrYyN1RxqE70SVubh5b2HYIgxX82TpvLhvp91shw7ebO6HrNOVk5DgDV4TNfNKhI3xnrlzPFn7GqWwnXkQxUne8G3fJp3kaCtbq_AimdgccQPXF5X42PcOGovniOdXknEH7XTm6HzOr-kFKlyXnw6X-WlVvxFe_pPr1_6KyoPh8Mf)

A sessão (login) está na memória local de cada servidor, com Round-Robin: o aluno faz login no Servidor 1 e a próxima requisição cai no Servidor 2, que pergunta "quem é você?". Correção: sessão fora do servidor (cache compartilhado ou banco) e servidores sem estado. Sticky Sessions é resposta parcial, se vier com o custo: se o servidor cai, a sessão some.

No simulador: os dois diagramas rodam a 100%, porque o simulador não sabe o que é uma sessão. O erro está no nome das caixas, não nos números. Pausar o Servidor 2 mostra outra coisa: o Servidor 1 aguenta uns 49 por segundo para 50 que chegam, e a fila cresce devagar.

## Diagrama C: "Sistema sem ponto único de falha"

Diagrama corrigido: [`01-ache-o-erro-C-gabarito.json`](01-ache-o-erro-C-gabarito.json) · [abrir no simulador](https://app.stackmonkey.dev/#d=z.vZS9jhMxEMdfxXKVE9Gx2VwiSAfU14SCAl0xsecSi7Vnsb0hIYrEQ_ACVIiC6sQT7JvckzDe3SQbokNUNKsd_-3xb768k2v0wZCTs9FQglcrE1HFyqOc7aQjjUHO3u-k0XImoagcBTmUcVuyLlVh0EW2Hdhkv2p0MbAQff1TVQVcsdjuumVXvIVKdLzmIbI1yrKhtGYzR2D3k9Z4503SkrWRM_5u5WycZfthB1EsTgAFgX4NBTiF_oTRrYAmLx6_fBUleAHRrOk5hhI9CEVWkLAYLAl0Gj3W30kMSARjGTqdsxRiu5NEZfkHzAZEqB9SRFAsiSlXli_zVDk9p4VJYX0i_4HTmeizln-cX0YAZTk6xRDQr43CE_7btJAYRg19QCswRKZKeatcNM2uchtXlO5U5FONXvRuH-VsdX5v2c6us8lQfqywwjdQgjJx2yWfCac3LWE-nvQJ838hzP8r4Xh6IgwY-o0IanXGF0L9C7kTGyFVkXsgmmLFjFcNMxinQeDGhIhNgdf4OZWXnaBbGpfceNQmXdLnHEoekDl3EzHfddYPqSn5HyFMuhBeTi-TrHuNrCHCAgL2m9gpEqU3tv7mDYlnwtc_ysIoEBpF18gDVT-UppmyI3XJnbtMCT8DnxzLkPeYpzcXxHki9tjcFJpHoTNi80hICFun2Pc9mIL47UjOR8d2P8SZSnXHUHrZvR_3nuzZC0LtKKdsdFo72HQYkKeUvK8cRokOLXGu5X_Rjuf04qlTSbnb738D)

O balanceador existe uma vez só; é ponto único de falha, e o nome afirma o contrário. Servidores e banco (com réplica de espera) estão duplicados; o balanceador não. Correção: dois balanceadores com um endereço que muda de dono quando o ativo para, ou o balanceador do provedor de nuvem, que já vem assim. Quem for além: o cache de sessões também existe uma vez só.

No simulador: pause o balanceador e o Success rate vai a zero. Pause o cache de sessões e acontece o mesmo. O StackMonkey não modela dois balanceadores com um endereço só, então o diagrama corrigido continua com uma caixa de balanceador, com a correção escrita no nome.

## Diagrama D: Cardápio da cantina, versão completa

Diagrama corrigido: [`01-ache-o-erro-D-gabarito.json`](01-ache-o-erro-D-gabarito.json) · [abrir no simulador](https://app.stackmonkey.dev/#d=z.ddLBTsMwDADQX4l82qQeum5MqDdA4jaB2BHt4CWmRKxJlqRlZarER_ADiAMfsj_hS0jbjW6auFRynNrPVrZQknVSK0hHEaDlz9IT94UlSLegtCAH6eMWpIAUcFUo7SACX5mQB76SpHyIFeZNfNXm2UBhSRkKbYfs5_2DJXHMkJNzIWe0ZUIiG-C6kGzUxo6yQgkdsUm4WNIbOZajdMNQuOswC4xQXhtS4cyip1aby80DYYCN4jiCDaThW0E6juM6OoiN6bmObCk59d55cyAaETE0K8lx97370mzA0Yrdp5G6G2B-f3fbNC6Ul-2PpvLPurFwbZsFBcyrti9hk5BeRodGsxAlEawLKugGDXLpqx47Ts65YtlrBXpcojviXqPi_-JIZVK1Nu181qhOHBd_1uTIOp2c8ZI9bzrpeYtQXmT7l_BkdX7yFvR-z80Uh2S3dt2NVC_q-hc)

Treze peças para 200 acessos por dia. Nenhuma peça tem um sintoma que a justifique: não há pico, não há leituras repetidas em escala, não há tarefa lenta, não há arquivo grande. Cada peça a mais é custo, operação e um modo de falha novo. Correção: voltar ao Exercício 2 (cliente, servidor, banco; 3 ou 4 peças). Regra 3: uma solução com 15 peças para 100 usuários está errada, mesmo que funcione.

No simulador: mesmo com 400 vezes a carga real, toda caixa fica perto de 0% de ocupação. O diagrama corrigido, com três caixas, mostra os mesmos 100% de sucesso. Monitoramento não existe como caixa no StackMonkey; os cartões e os gráficos do simulador fazem esse papel.
