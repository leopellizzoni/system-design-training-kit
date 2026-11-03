# Resposta 3: Inscrições da Semana Acadêmica

Diagrama esperado: [`03-inscricoes-da-semana-academica-gabarito.json`](03-inscricoes-da-semana-academica-gabarito.json) · [abrir no simulador](https://app.stackmonkey.dev/#d=z.vZTNitRAEMdfpWkQZiFIktkZNDdd8La47CIeZA81nTLTmHTF7s64wzDgQ_gOPoF4d9_EJ7E6H5tkhxVPXgLV_-6qX33lIHdonSYjsySSYNVWe1S-sSizgzSUo5PZh4PUucwklI0hJyPp9zXrUpUajWfbQBXsV60uFgZ2WEBO9kz8_vpNrGKhjVNW33-__4FO1GSFw6IxOfHbzsklR2IPVKPhMwuerVUcyUrfvbc6WEnM5p3M-LuX2TKOj1GPVW5GpJIgfw0lGIV2BOtPAlJL5LTxVEEmEgaxOx3OQXCEZyJHcXH1Tvz6KW6u3r5hF1AWxATbiv1YYuhr2ugA-YXsJ65d4OzRlukpHNR1MuK10RSOZDdD-KQDw0qg89BWxjbG6_ZWvfdbCjEV2dCQZBL9RTR4vWQrjeTnBhu8gBqU9vuxbuvzDi5drqZw6b_Apf8Lbrke4Ry66ayB2s7QnGuHadEKQlFVg_W63DLe2bzJJRXaCEcVCiDhLSmwoc9D69krGr4S_FrMdYg6oWYyXopr8JoY-Hk8SS9pO_8op1Wf08v1acHzyajm4GEDDqdjahSJxXRZulTCKAoUBdgCSgrsGK54mKLX5HwROjCjXz30JZ2Ar89PsNNH2KEVt-w-L_pfwEdL1ewnQN3uheR6rdtEGsb-KSWdKsOC0NDyuZb-RXt4l2-eehWU2-PxDw)

Alunos → Balanceador (Round robin) → Servidor 1 e Servidor 2 (sem estado) → Banco. Os dois servidores ligados a um cache compartilhado de sessões. Sessão no banco ou em token assinado também vale, desde que fora do servidor.

No simulador: o diagrama de partida mostra o servidor único a 100% de CPU, fila cheia, latência de segundos e Success rate em torno de 70%. Com o balanceador e dois servidores, cada um fica perto de 60% e o Success rate vai a 100%.

**Previsão.** Em um balanceador real, nenhuma requisição dá erro para o usuário: ele tenta o outro servidor e, depois de alguns erros seguidos, para de mandar ao que caiu. Na prática com nginx, o cabeçalho X-Atendido-Por mostrava "5002, 5001". A resposta mais comum é "metade dá erro"; não é o que acontece com nginx.

**Falha.** No simulador, pausar o Servidor 2 mostra justamente a resposta errada: o balanceador do StackMonkey não tem health check, continua mandando metade das chamadas para a caixa pausada, e o Success rate cai para cerca de 55%. É o que acontece com um balanceador sem health check. Para ver o comportamento do nginx, apague a ligação Balanceador → Servidor 2 em vez de pausar: todo o tráfego vai ao Servidor 1, o sistema continua, com metade da capacidade (40 req/s para 50 req/s de carga), e a fila volta a crescer, mais devagar.

**Pergunta 1.** Fora do servidor: cache compartilhado, banco ou token assinado. Na memória, o aluno faz login no Servidor 1 e a próxima página cai no Servidor 2, que não o conhece. Sticky Sessions é o atalho: resolve enquanto o servidor estiver vivo; se ele cai, a sessão some. O simulador não mostra nada disso: sessão é lógica, não carga.

**Pergunta 2.** O balanceador, se for único, e o banco. Pause o balanceador e o Success rate vai a zero.

**Pergunta 3.** Sim, provavelmente: inscrição é escrita, e escrita não se divide com réplica. Se o banco aguenta 30 inscrições por segundo, dobrar os servidores não passa de 30. No simulador o banco está folgado porque foi configurado assim; suba o tráfego para 5x e veja quem estoura primeiro.

**Conta (opcional).** 50 ÷ (40 × 0,7) ≈ 1,8: 2 servidores com 30% de folga; 3 para tolerar a queda de um.

**Erros comuns.**

- Pôr cache de dados, fila e CDN já aqui: desproporcional ao sintoma.
- Sticky Sessions apresentado como solução da sessão, sem o modo de falha.
- Não dizer onde fica a sessão.
- Achar que o balanceador não é ponto único de falha.
