# fluxo-de-trabalho-de-Nathan

# Automação de Relatório Semanal de Vendas (Nathan's Workflow)

Este repositório contém o código-fonte de um workflow desenvolvido na plataforma n8n. O objetivo deste software é automatizar a geração e distribuição do relatório semanal de vendas da ABCorp, eliminando tarefas manuais, repetitivas e propensas a erros.

## Resumo da Solução

Este sistema foi projetado para resolver um desafio específico do gerente de análise, Nathan. Ele automatiza a coleta de dados de um data warehouse legado, processa e filtra os pedidos de venda com base em seu status, e distribui as informações relevantes para as equipes corretas através de diferentes canais de comunicação.

## Como Funciona

O workflow é orquestrado pelo n8n e segue uma lógica de negócios clara para garantir que cada tipo de pedido receba o tratamento adequado.

1.  **Coleta de Dados (`HTTP Request`)**
    *   O processo inicia buscando todos os registros de vendas do data warehouse interno da ABCorp através de um endpoint de API.
    *   Esta etapa substitui a extração manual de dados, garantindo consistência e agilidade.

2.  **Filtragem e Roteamento (`If`)**
    *   O workflow analisa cada pedido individualmente para verificar seu `orderStatus`.
    *   **Se o status for `processing`**, o pedido é enviado para o "Caminho A" (tratamento de pedidos em processamento).
    *   **Se o status for `booked`**, o pedido é enviado para o "Caminho B" (cálculo de vendas concluídas).

3.  **Caminho A: Preparação de Dados para Vendas (`Edit Fields` & `Airtable`)**
    *   **Seleção de Dados:** Apenas os campos essenciais para a equipe de vendas (`orderID` e `employeeName`) são selecionados, otimizando o processo.
    *   **Armazenamento:** Os dados filtrados são inseridos em uma tabela dedicada no Airtable chamada `processingOrders`, servindo como uma lista de trabalho para os gerentes de vendas acompanharem os clientes.

4.  **Caminho B: Cálculo e Notificação (`Code` & `Discord`)**
    *   **Cálculo Customizado:** Um script JavaScript customizado processa todos os pedidos com status `booked` para calcular duas métricas chave: o número total de pedidos (`totalBooked`) e a soma total de seus valores (`bookedSum`).
    *   **Notificação:** Um resumo com os resultados é enviado automaticamente para um canal no Discord, mantendo a equipe informada sobre o desempenho das vendas da semana.

## Como Usar

1.  Faça o download do arquivo `fluxo de trabalho de Nathan.json` deste repositório.
2.  Em sua instância do n8n, utilize a função `Import from File` para carregar o workflow.
3.  Configure as seguintes credenciais:
    *   **HTTP Request:** Crie uma credencial do tipo "Header Auth" com os dados de acesso ao data warehouse.
    *   **Airtable:** Crie uma credencial "Personal Access Token" com sua chave do Airtable.
    *   **Discord:** Crie uma credencial "Webhook" com a URL do canal desejado.
4.  Verifique se a Base e as Tabelas no nó do Airtable correspondem à sua configuração.
5.  Ative o workflow.

## Propriedade Intelectual

Este software foi desenvolvido por **Alexandre P Calaes (usuário `Acalaes` no GitHub). O histórico de versões e o resumo hash de cada alteração estão registrados nos commits deste repositório Git, servindo como prova de anterioridade para fins de registro de propriedade intelectual.
