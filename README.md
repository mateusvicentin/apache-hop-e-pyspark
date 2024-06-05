<h1 align="center">Acessando arquivos do MongoDB via Apache Hop e Spark</h1>

<p>Nesse projeto, utilizaremos o projeto <a href="https://github.com/mateusvicentin/cluster-mongodb" target="_blank">Cluster MongoDB</a> para conectar em tempo real via Apache Hop e realizar operações dentro do ambiente. Além disso, faremos o download de um CSV dos arquivos para realizar operações similares com Apache Spark.</p>

<h2>Apache Hop</h2>

<h4>Para a conexão no Apache Hop, utilizaremos quatro ações:</h4>
<ul>
  <li>Conexão com o banco MongoDB</li>
  <li>Selecionar os valores</li>
  <li>Ordenar os valores</li>
  <li>Agrupar os valores</li>
</ul>

<h4>Conexão com o banco MongoDB</h4>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/04486684-de4d-4ec1-9cbd-026af608f3a0" alt="img7">
</p>
<p>Procuramos pela ação chamada "MongoDB Input" e a nomeamos como "conexão_mongo_DB". Para conectá-la ao banco, escolhemos a opção "Create New Metadata Element" e realizamos a seguinte configuração:</p>
<ul>
  <li>Hostname: localhost</li>
  <li>Port: a porta utilizada, no caso, 27018</li>
</ul>
<p>Após isso, selecionamos "Get Databases" para conectar ao banco e buscar todos os bancos de dados. Caso esteja tudo certo, veremos a seguinte tela:</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/e5888f60-f8c4-4e79-9283-6a0e88c73c1f" alt="img1">
</p>
<p>Selecionamos o banco de dados desejado, neste caso, "vicentin_filial_D".</p>
<p>Em "MongoDB Connection Name", definimos um nome para facilitar a identificação durante a conexão. Usaremos o mesmo nome do banco:</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/48ccc99e-7b7f-4996-bb42-2ee5fd11eb37" alt="img2">
</p>
<p>Depois, clicamos em "Get Collections" para buscar as coleções presentes no Database:</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/606d03bd-46fa-465f-85f5-a247818265c3" alt="img3">
</p>
<p>Para buscar todos os campos, vamos para a opção "Fields" e, após isso, em "Get Fields". Caso tudo esteja correto, veremos uma tela parecida com esta:</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/ebd42879-8298-42cb-b2f2-60029db426c6" alt="img4">
</p>
<p>Executamos a ação e verificamos se está retornando as informações em "View Output":</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/25f6ba2a-03bd-444a-bc81-79a5368a1522" alt="img5">
</p>
<p>Ele está lendo um arquivo JSON diretamente do MongoDB e conectado em tempo real com o banco. Qualquer alteração no banco será atualizada ao executar a ação.</p>
<p>Neste exemplo, vamos preparar para somar a quantidade total de cada produto. Por exemplo, o banco está da seguinte forma:</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/93c5fae3-9ec5-4d46-b4bb-d7b39ba6a9a6" alt="img6">
</p>
<p>A ideia é procurar todos os produtos com o nome "Hortelã" e somar o campo "quantidade" para produtos com o mesmo nome. Para isso, escolhemos a ação chamada "Select Values".</p>

<h4>Selecionar os valores</h4>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/74feb58e-a817-45ca-819f-54347fa2f9d1" alt="img8">
</p>
<p>Selecionamos apenas "nome" e "quantidade" para evitar interferências ou problemas com a consulta, ficando dessa forma:</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/13178574-37b2-4b32-a6b5-3ca79d144e3d" alt="img9">
</p>
<p>Após executar a ação novamente, consultamos o "View Output":</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/de6f553a-27b8-4231-812d-e1e410064c70" alt="img9">
</p>
<p>Ele já está retornando apenas o nome e a quantidade de produtos, lembrando que há nomes repetidos com quantidades diferentes. No exemplo abaixo, ele mostra apenas 100 linhas de um total de 1.000.000.</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/cb714081-9482-4479-83f5-94674477efea" alt="img10">
</p>

<h4>Ordenar os valores</h4>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/96cf0ab5-f64d-471a-b717-291faf5e107f" alt="img11">
</p>
<p>Nesta ação, ordenamos as duas colunas, mas apenas pelo nome, de A a Z:</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/b2a87ead-9a92-4af4-b98c-f7aadf59ed00" alt="img12">
</p>
<p>Após executar a ação novamente, consultamos o "View Output", que mostrará apenas as 100 primeiras linhas:</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/9954002c-32f5-4b81-8776-914dcaae212e" alt="img13">
</p>
<p>Para continuar, utilizamos a última ação deste projeto, realizando um Group By dessas duas informações com a ação "Group By".</p>

<h4>Agrupar os valores</h4>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/e5f7ef7c-d118-43b7-ab20-385b95b38612" alt="img14">
</p>
<p>Agora agrupamos pelo nome, somando as quantidades dos produtos com nomes iguais:</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/6dcda093-fb3c-4f30-a609-bfdb9dca96a0" alt="img15">
</p>
<p>Após executar a ação novamente, consultamos o "View Output":</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/f34c3234-f75c-46b9-bc88-df75a511ab0a" alt="img16">
</p>
<p>Para melhorar a visualização, utilizamos mais um "Sort Rows" para mostrar o produto com maior quantidade no estoque:</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/49a76f8a-8f5b-42b8-9cdd-62ecc3798498" alt="img17">
</p>
<p>Configuramos a coluna "count" criada no "Group By":</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/5d4b4823-ba4b-4033-8708-cd9dab17fc67" alt="img18">
</p>
<p>No Database "vicentin_filial_D" e a collection "produtos_estoque_A", o produto "Shampoo" é o que mais tem quantidade:</p>
<p>O pipeline completo com todas as ações ficará assim:</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/2cebedbb-ad52-47cf-a484-ef4dcac38f55" alt="img19">
</p>

<h4>Atualizando a quantidade do produto "Shampoo" e executando o Pipeline novamente</h4>
<p>Para atualizar a quantidade do produto "Shampoo", utilizaremos o ID para alterar apenas uma informação específica. Neste caso, o ID é 77939316.</p>

```python
db.produtos_estoque_A.find({nome: "Shampoo"})
```
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/0fe96945-41a7-42e9-aa1c-1a26c052f10d" alt="img20">
</p>

```python
db.produtos_estoque_A.updateOne(
   { id: 77939316 },
   { $set: { quantidade: 100 } }
)
```
<p>Após consultar novamente o mesmo ID:</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/a23f5107-8874-43d0-91b1-f1ee3b8e14d2" alt="img21">
</p>
<p>Executamos nosso Pipeline e verificamos a consulta final:</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/c24fc306-421a-4787-9566-e9442cb48687" alt="img22">
</p>
<p>Como adicionamos mais 40 Shampoos na quantidade, ele mostra esses 40 produtos a mais.</p>
<h4>Log de operações</h4>
<p>Todo processo tem um log das operações, que pode ser verificado caso ocorra algum erro:</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/eb7d522a-41a1-4531-b6d6-31105c1f3913" alt="img23">
</p>
<h2>Concatenando com outro Database</h2>
<h4>Vamos concatenar com a vicentin_filial_E</h4>
<p>Para isso, aproveitamos o Pipeline anterior e adicionamos uma nova ação chamada "Concat Fields" e outra de "MongoDB Input".</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/e85379d7-61f8-4e43-8a83-742bbe715adb" alt="img24">
</p>
<p>Configuramos o "MongoDB Input" com as informações do "vicentin_filial_E":</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/28fa3413-7654-49f7-a572-f328649fefce" alt="img25">
</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/277ba7a4-566f-47c9-9073-f0bd37170b35" alt="img25">
</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/85522947-42c3-48ac-9a3f-2d29a2bcbc36" alt="img26">
</p>
<p>Ligamos os dois "MongoDB Input" com o "Concat Fields":</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/9abe7054-9e8b-4847-95be-122c940b6641" alt="img27">
</p>
<p>A configuração ficará assim:</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/b524a640-9e29-450a-a690-f6427eb6f504" alt="img28">
</p>
<p>O pipeline agora ficará assim:</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/020e1189-b5d0-4f32-9022-2c228f8c513a" alt="img29">
</p>
<p>Após consultar os dados novamente:</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/2f9fb7ec-4dcc-4851-a272-d18e9e730cb6" alt="img30">
</p>
<h4>Log de operações</h4>
<p>O log ficará assim agora:</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/23cd908e-6285-44a1-8123-2731fe62dde6" alt="img31">
</p>
<ul>
    <li>A conexão com o MongoDB da filial D tem 1.000.000 de produtos.</li>
    <li>A conexão com o MongoDB da filial E tem 1.000.000 de produtos.</li>
    <li>O processo de concatenação está recebendo esses arquivos e juntando-os em 2.000.000 de registros.</li>
    <li>O processo de ordenação de linhas (SortRows) está agrupando os produtos, resultando em 111 produtos com nomes únicos.</li>
    <li>O processo de ordenação (OrderBy) está retornando esses 111 produtos em ordem decrescente.</li>
</ul>
<p>Com isso, encerramos o processo de fazer esse agrupamento pelo Apache Hop. Caso queira adicionar mais filiais, basta adicionar mais uma ação de "MongoDB Input" e ligá-la à ação "Concat Field".Isso e uma das diversas funções presentes no Apache Hop</p>

<h2>Apache Spark</h2>
<p>Agora vamos realizar o mesmo procedimento usando Apache Spark.</p>

<h4>Iniciando o Serviço Spark</h4>
<p>Vamos começar importando o SparkSession e carregando o CSV da "vicentin_filial_D".</p>

```python
import findspark
findspark.init()

from pyspark.sql import SparkSession
spark = SparkSession.builder\
        .master('local')\
        .appName('sparkcollab')\
        .getOrCreate()
        
df = spark.read.csv("C:\\Users\\Vicentin\\Documents\\Estudos\\Dados\\CSV\\vicentin_filial_D.csv", encoding='utf-8', header=True, inferSchema=True, sep=',')
df.show(truncate=False)
```
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/bf185b69-28e5-4afc-b6ba-97e8ab34566e" alt="img32">
</p>
<h4>Importando Bibliotecas</h4>
<p>Vamos importar as bibliotecas necessárias para as consultas.</p>

```python
from pyspark.sql.functions import count, col, asc, desc, sum, year, month
```
<h4>Verificando o Schema da tabela</h4>

```python
df.printSchema()
```
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/c4809764-b2a2-4e30-96be-4d420ca6feed" alt="img33">
</p>
<p>Podemos visualizar a estrutura da tabela e os tipos de dados de cada coluna.</p>
<h4>Filtrando apenas as colunas desejadas</h4>
<p>Vamos criar um DataFrame selecionando apenas as colunas "nome", "quantidade" e "preco_compra".</p>

```python
df_quantidade_produtos = df.select('nome', 'quantidade', 'preco_compra')
df_quantidade_produtos.show(truncate=False)
```
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/177a4fb0-14a5-41cb-ad39-92fd551d8233" alt="img34">
</p>
<h4>Dataframe com a soma dos produtos e suas quantidades</h4>
<p>Vamos agrupar os produtos pelo nome e somar as quantidades.</p>

```python
df_soma = df_quantidade_produtos.groupBy('nome').sum('quantidade').orderBy(col('sum(quantidade)').desc())
df_soma.show(1000, truncate=False)
```
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/53e20b73-bde9-4048-95f5-70def52a0d9d" alt="img35">
</p>
<h4>Somando a quantidade total de produtos</h4>
<p>Vamos calcular a quantidade total de produtos em estoque.</p>

```python
df_soma_total = df_soma.groupBy().sum('sum(quantidade)').alias('Quantidade total de Produtos em Estoque')
df_soma_total.show(truncate=False)
```
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/4489ba68-5ec1-46ba-a130-1b90a08f6075" alt="img36">
</p>
<h4>Criando as colunas 'year' e 'month'</h4>
<p>Vamos separar a coluna 'data_entrada' por ano e mês para facilitar consultas.</p>

```python
df = df.withColumn('month', month(df['data_entrada']))
df = df.withColumn('year', year(df['data_entrada']))
df_mes_ano_produto = df.select('nome', 'quantidade', 'preco_compra', 'month', 'year')
df_mes_ano_produto.show(truncate=False)
```
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/67f67884-a870-4564-9d37-b35e135749ab" alt="img38">
</p>
<h4>Agrupando a quantidade total de produtos por ano</h4>
<p>Vamos agrupar os produtos pelo ano de entrada.</p>

```python
df_ano = df.groupBy('year').sum('quantidade').orderBy(col('sum(quantidade)').desc())
df_ano.show(truncate=False)
```
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/83abfd2b-f3fd-4d3a-b2ab-c7d784a97194" alt="img39">
</p>
<p>Só temos produtos cadastrados nos anos de 2023 e 2024. Podemos adicionar o mês na consulta também.</p>

```python
df_ano = df.groupBy('year','month').sum('quantidade').orderBy(col('sum(quantidade)').desc())
df_ano.show(truncate=False)
```
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/c6749bc0-9604-4fb5-bbc3-cb1638466613" alt="img40">
</p>
<p>Com isso, concluímos o processo de transformação usando o Spark para visualização dos dados.</p>
