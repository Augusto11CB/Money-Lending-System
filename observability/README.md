- Taxa de Sucesso: Porcentagem de solicitações que a API processa com sucesso.
    - Alerta New Relic para acompanhar se essa porcentagem fica abaixo de XX% durante um certo periodo.

- Latência: Tempo que leva para a API processar uma solicitação.
    - Alerta New Relic para verificar se o response time médio fica acima de certo valor XX durante um certo periodo de tempo.

- Throughput: Número de solicitações que a API pode processar por unidade de tempo.
    - Alerta New Relic para verificar se o throuhput de solicitações não está dentro de um intervalo médio (muito abaixo ou muito acima pode representar problemas). 
    - New Relic possui tools baseada em histórico que nos permite extrair essa informação.

- Erros: Número de solicitações que falham ou resultam em erros (New Relic).
    - Alerta New Relic para monitorar número de erros:
        - 4XX durante um certo periodo.
        - 5XX durante um certo periodo.

- Uso de Recursos: Quantidade de CPU, memória e outros recursos que a API consome (Cloud Watch).