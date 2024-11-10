# Case API Fake Store | Enjoei/Elo7
Este repositório contém uma solução para o case de Engenharia de Dados Júnior da Enjoei/Elo7, que consiste em consumir dados de uma API, processar e transformar as informações e gerar uma saída em formato JSON. O foco principal é coletar informações relevantes de usuários e carrinhos de compras, incluindo a data mais recente de adição ao carrinho e a categoria com maior número de produtos adicionados.

### Tópicos 
:small_blue_diamond: [Descrição do Projeto](#descrição-do-projeto)

:small_blue_diamond: [Tecnologias Utilizadas](#tecnologias-utilizadas)

:small_blue_diamond: [Estrutura do Código](#estrutura-do-código)

:small_blue_diamond: [Funcionalidades](#funcionalidades)

:small_blue_diamond: [Seções Principais](#seções-principais)

:small_blue_diamond: [Saída do arquivo JSON](#saída-do-arquivo-json)

:small_blue_diamond: [Análise de Eficiência e Melhorias](#análise-de-eficiência-e-melhorias)

:small_blue_diamond: [Agradecimentos](#agradecimentos)

## 🎯Descrição do Projeto
Esse projeto é uma solução para o case de engenharia de dados, onde foi necessário:
- Consumir dados de dois endpoints diferentes da API Fake Store.
- Transformar e manipular os dados para extrair informações sobre cada usuário e seus consumos.
- Persistir os dados em um arquivo JSON.

## 🛠️Tecnologias Utilizadas
- Python: linguagem de programação escolhida por sua versatilidade e manipulação de dados.
- Pandas: utilizado para estruturação e manipulação dos dados.
- Requests: para consumo da API Fake Store.
- Collections: especificamente o `defaultdict` para gerenciar e agrupar dados com eficiência.
- JSON: Para o arquivo final de saída.

## 📄Estrutura do Código
- Consumo de dados: Requisições aos endpoints `/carts` e `/products`.
- Transformação de dados: Extração e cálculo das informações de interesse.
- Persistência: Salvamento dos dados processados em um arquivo JSON.

## 🚀Funcionalidades
Abaixo estão os passos principais e funcionalidades do código.

### Requisição dos Dados:
Fiz a conexão à Fake Store API para buscar informações de carrinhos e produtos.

### Transformação dos Dados:
Realizei o mapeamento de cada `id` de produto para sua categoria correspondente.
Processei cada carrinho para identificar:
- A data mais recente de adição ao carrinho para cada usuário.
- A categoria de produto mais adicionada no carrinho de cada usuário.

### Geração do Arquivo JSON:
Salvei o resultado final em um arquivo JSON, contendo:
-   `user_id`: identificador único do usuário.
-   `ultima_data`: última data de adição de produto ao carrinho.
-   `categoria_mais_produtos`: categoria com maior quantidade de produtos adicionados.

## ▶️Seções Principais
- Requisição de Dados: Verifiquei o status de cada requisição e preparei uma mensagens de erro em caso de falha.
- Mapeamento e Processamento: Utilizei um `defaultdict` para mapear cada `user_id` com a última data de adição ao carrinho e a categoria com o maior número de produtos.
- Preparação e Salvamento do Resultado Final: Com os dados processados, criei um `DataFrame` e o salvei em JSON, facilitando o uso em outras ferramentas.

## 💾Saída do arquivo JSON
A saída gerada será um arquivo JSON com o seguinte formato:

    [{"user_id":1,"ultima_data":"2020-03-02T00:00:00","categoria_mais_produtos":"men's clothing"},{"user_id":2,"ultima_data":"2020-03-01T00:00:00","categoria_mais_produtos":"men's clothing"},{"user_id":3,"ultima_data":"2020-03-01T00:00:00","categoria_mais_produtos":"men's clothing"},{"user_id":4,"ultima_data":"2020-03-01T00:00:00","categoria_mais_produtos":"electronics"},{"user_id":8,"ultima_data":"2020-03-01T00:00:00","categoria_mais_produtos":"women's clothing"}]

O arquivo JSON contém informações agregadas de cada usuário, resumindo a última data de adição ao carrinho e a categoria mais popular.

## ⚠️Análise de Eficiência e Melhorias
- Eficiência: O uso de `defaultdict` reduz a complexidade ao contar produtos por categoria, tornando o processamento de dados mais eficiente.
- Escalabilidade: A abordagem atual é eficaz para o volume de dados da API, mas, para grandes volumes seria bom considerar o uso de frameworks de processamento em lote.
- Tratamento de Erros: Mensagens são exibidas para sinalizar qualquer problema de conexão com a API.
- Documentação: Cada seção do código inclui comentários que explicam o propósito de cada bloco, visando facilitar a manutenção e a extensão da solução.

## 😊Agradecimentos
Agradeço à Enjoei/Elo7 pela oportunidade de realizar este case. Estou entusiasmado com a possibilidade de contribuir para o time de engenharia de dados e explorar soluções inovadoras na área de dados.
Agradeço mais uma vez pela oportunidade e fico à disposição para qualquer etapa adicional do processo.

## Licença
Este projeto está sob a Licença MIT.
