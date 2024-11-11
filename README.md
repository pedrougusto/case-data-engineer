# Case API Fake Store | Enjoei/Elo7
Este repositório contém uma solução para o case de Engenharia de Dados Júnior da Enjoei/Elo7, que consiste em consumir dados de uma API, processar e transformar as informações e gerar uma saída em formato JSON. O foco principal é coletar informações relevantes de usuários e carrinhos de compras, incluindo a data mais recente de adição ao carrinho e a categoria com maior número de produtos adicionados.

### Tópicos: 
:small_blue_diamond: [Descrição do Projeto](#descrição-do-projeto)

:small_blue_diamond: [Tecnologias Utilizadas](#tecnologias-utilizadas)

:small_blue_diamond: [Estrutura do Código](#estrutura-do-código)

:small_blue_diamond: [Funcionalidades](#funcionalidades)

:small_blue_diamond: [Saída do arquivo JSON](#saída-do-arquivo-json)

:small_blue_diamond: [Análise de Eficiência](#análise-de-eficiência)

:small_blue_diamond: [Agradecimentos](#agradecimentos)

## 🎯Descrição do Projeto
Esse projeto é uma solução para o case de engenharia de dados, onde foi necessário:
- Consumir dados de dois endpoints diferentes da API Fake Store.
- Transformar e manipular os dados para extrair informações sobre cada usuário e seus consumos.
- Persistir os dados em um arquivo JSON.

## 🛠Tecnologias Utilizadas
- **Python**: linguagem de programação escolhida por sua versatilidade e manipulação de dados.
- **Pandas**: utilizado para estruturação e manipulação dos dados.
- **Requests**: para consumo da API Fake Store.
- **Collections**: especificamente o `defaultdict` para gerenciar e agrupar dados com eficiência.
- **JSON**: Para o arquivo final de saída.

## 📄Estrutura do Código
- **Consumo de dados**: Requisições aos endpoints `/carts` e `/products`.
- **Transformação de dados**: Extração e cálculo das informações solicitadas no case.
- **Persistência**: Salvamento dos dados processados em um arquivo JSON.

## 🚀Funcionalidades
Abaixo estão os passos principais e funcionalidades do código.

### 📍Requisição dos Dados:
Fiz a requisição à Fake Store API para buscar informações de carrinhos e produtos.

    url_carrinho = "https://fakestoreapi.com/carts"
    response_carrinho = requests.get(url_carrinho)

    url_produtos = "https://fakestoreapi.com/products"
    response_produtos = requests.get(url_produtos)

Após cada requisição, verifico se a resposta foi bem-sucedida `status 200`. Se houver erro, exibo uma mensagem de alerta.

    if response_carrinho.status_code == 200:
    dados_carrinho = response_carrinho.json()
    else:
    print("Erro ao acessar a API de carrinhos")
    
    if response_produtos.status_code == 200:
    dados_produtos = response_produtos.json()
    else:
    print("Erro ao acessar a API de produtos")

### 📍Transformação dos Dados:
Utilizei um `defaultdict` para mapear cada `user_id` e processei cada carrinho para identificar:
- A data mais recente de adição ao carrinho para cada usuário.
- A categoria de produto mais adicionada no carrinho de cada usuário.

Mapeamento do ID do produto para a categoria e dicionário para armazenar o resultado final:

    produto_para_categoria = {produto['id']: produto['category'] for produto in dados_produtos}
    dados_finais = defaultdict(lambda: {'ultima_data': None, 'categorias': defaultdict(int)})

Processando cada carrinho para extrair as informações:

    for carrinho in dados_carrinho:
    user_id = carrinho['userId']
    date_str = carrinho['date'].split('.')[0].rstrip('Z')
    data = datetime.fromisoformat(date_str)

Atualizando a data mais recente de adição ao carrinho:
    
    if dados_finais[user_id]['ultima_data'] is None or data > dados_finais[user_id]['ultima_data']:
        dados_finais[user_id]['ultima_data'] = data

Contando os produtos por categoria:
    
    for produto in carrinho['products']:
        categoria = produto_para_categoria[produto['productId']]
        dados_finais[user_id]['categorias'][categoria] += produto['quantity']

### 📍Preparação do Resultado Final:
- Após processar todos os carrinhos, percorro os dados finais de cada usuário para identificar a categoria com o maior número de produtos adicionados.
- Crio uma lista chamada `resultado_final`, onde cada entrada contém o `user_id`, a `ultima_data` e a `categoria_mais_produtos` do usuário.

Preparando a saída final com a categoria mais frequente:

    resultado_final = []
    for user_id, info in dados_finais.items():
    categoria_mais_freq = max(info['categorias'], key=info['categorias'].get)
    resultado_final.append({
        "user_id": user_id,
        "ultima_data": info['ultima_data'].isoformat(),
        "categoria_mais_produtos": categoria_mais_freq

### 📍Geração do Arquivo JSON:
Com os dados processados, criei um `DataFrame` e o salvei em JSON, facilitando o uso em outras ferramentas.
O arquivo contém:
-   `user_id`: identificador único do usuário.
-   `ultima_data`: última data de adição de produto ao carrinho.
-   `categoria_mais_produtos`: categoria com maior quantidade de produtos adicionados.

Criando o dataFrame com o resultado final:
        
    df = pd.dataFrame(resultado_final)

Salvando no formato JSON:

    df.to_json("Case Data Engineer I - Pedro Augusto.json", orient="records")

## 💾Saída do arquivo JSON
A saída gerada será um arquivo JSON com o seguinte formato:

    [{"user_id":1,"ultima_data":"2020-03-02T00:00:00","categoria_mais_produtos":"men's clothing"},{"user_id":2,"ultima_data":"2020-03-01T00:00:00","categoria_mais_produtos":"men's clothing"},{"user_id":3,"ultima_data":"2020-03-01T00:00:00","categoria_mais_produtos":"men's clothing"},{"user_id":4,"ultima_data":"2020-03-01T00:00:00","categoria_mais_produtos":"electronics"},{"user_id":8,"ultima_data":"2020-03-01T00:00:00","categoria_mais_produtos":"women's clothing"}]

O arquivo JSON contém informações agregadas de cada usuário, resumindo a última data de adição ao carrinho e a categoria mais popular.

## ✅Análise de Eficiência
- **Eficiência**: O uso de `defaultdict` reduz a complexidade ao contar produtos por categoria, tornando o processamento de dados mais eficiente.
- **Escalabilidade**: A abordagem atual é eficaz para o volume de dados da API, mas, para grandes volumes seria bom considerar o uso de frameworks de processamento em lote.
- **Tratamento de Erros**: Mensagens são exibidas para sinalizar qualquer problema de conexão com a API.
- **Documentação**: Cada seção do código inclui comentários que explicam o propósito de cada bloco, visando facilitar a manutenção e a extensão da solução.

## 😊Agradecimentos
Agradeço à equipe Enjoei/Elo7 pela oportunidade de realizar este case, estou entusiasmado com a possibilidade de contribuir para o time de engenharia de dados e explorar soluções inovadoras na área de dados. Agradeço mais uma vez pela oportunidade e fico à disposição para qualquer etapa adicional do processo.

## Licença
Este projeto está sob a **Licença MIT**.
