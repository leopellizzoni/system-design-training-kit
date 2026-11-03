# Exercício extra: Certificados do evento

Arquivo: [`extra-certificados-do-evento.json`](extra-certificados-do-evento.json) · [abrir no simulador](https://app.stackmonkey.dev/#d=z.zZTBjtMwEIZfxfJlWymgxN1WkCMgbiutlgMHtAfXnqYWicc4btmqqsRD8A48CG_CkzBO0o1DVcGRS5SZP_H_ze84R74H3xq0vCwyLr3amgAq7Dzw8sgtamh5-enIjeYld9IHnvFwcKRyVRuwsbayifU9qUYZJ22Als1IVrJh0LAbaEwwnimgBzbU1XgzZ7--fWdL5tCzFqqd1Ugr9UvekSuthw4s9bwMVC0z3pinj97EosjzjD_xkq4HXi7y_JQNhPV65KtR6jeylpZ8R8qhQwyxKesKac1tQ4pHonjAtYmuX9F_pmDIOB_MFuLSTjpXjIYt-L1RMHp9iA0yYkU3bQVeMmT3794zYGD3JlbwopGmZn1oGpiH1qHVHbLf2WC6ldwhbDFyKfRxR4qE8FV2dr6LFVF-2cEO3konlQmHMa7VbT-BWCzTCcS_TCD-qwkWq3ECnWy5lkGuZQvpdluFbOYm3yakn2I7p6fBVsZ2mNiGKgJOkJbP2CLBXt1egIoB9PXqMmqIMf0t658_MGY4ZDqj2w16OZ9EWeEYY57wdCkl1FGTJsRb0X3Ff8BSZ0MuD90By1_mYoIeM36kZHQ1_AI2HpvkJ4D9cYvjDUp_-PB8Lq4pIlXOJwj7nZwq4qry_E4f6pXXBvHxdPoN)

**Cenário.** Ao fim da Semana Acadêmica, 5.000 participantes recebem um certificado em PDF por e-mail. No simulador: 5 pedidos por segundo.

**Sintoma.** Quem clica em "emitir certificado" espera dezenas de segundos (o servidor gera o PDF e envia o e-mail antes de responder), e o site fica lento para todo mundo.

**O que você recebe.** Participantes → Balanceador → Servidor 1 e Servidor 2 → Banco. Os servidores chamam o serviço de e-mail diretamente, dentro do clique. Deixe rodar e veja a latência e o Success rate.

## Tarefa

1. Faça o clique responder na hora e o certificado chegar depois. Caixas esperadas: uma **Queue**, um ou mais **Worker** e um **Object storage** para os PDFs.
2. Mova a geração do PDF e o envio do e-mail para o worker: a ligação com o serviço de e-mail sai dos servidores e passa a sair do worker. O servidor de aplicação só grava o pedido, publica na fila e responde "pedido recebido; o certificado chega por e-mail".
3. Marque no nome de cada caixa se ela está no caminho síncrono (o participante espera) ou assíncrono (não espera).

## Previsão (antes de mexer)

O que o participante vê 0,1 segundo depois de clicar? E 2 minutos depois?

## Falhas para testar

1. Pause o worker por um tempo. Os pedidos se perdem? Onde eles ficam? O participante percebe?
2. Um worker cai depois de enviar o e-mail e antes de avisar que terminou. O participante recebe dois e-mails? O que o worker precisa checar antes de enviar? Isso o simulador não mostra: é lógica.

## Perguntas

1. Por que o PDF vai para o armazenamento de objetos e não para o banco?
2. Se chegam mais pedidos do que os workers dão conta, o que acontece? Como você saberia? Teste com 2x de tráfego.

Pronto quando: o diagrama separa síncrono e assíncrono; você sabe explicar as duas falhas; o PDF está fora do banco.
