<h1 align="center">Acessando arquivos do MongoDB via Apache Hop e Spark</h1>
<p>Nesse projeto iremos aproveitar o projeto <a href="https://github.com/mateusvicentin/cluster-mongodb" target="_blank">Cluster MongoDB</a> iremos conectar em tempo real via Apache Hop e realizar operações dentro do ambiente, e baixar um CSV dos arquivos e realizar operações parecidas como Apache Spark.</p>

<h2>Apache Hop</h2>
<h4>Para conexão no Apache Hop, iremos utilizar 4 ações</h4>
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
<p>Vamos procurar pela ação chamada "MongoDB Input" e irei nomeá-la como "conexão_mongo_DB". Para conectá-la com o banco, escolheremos a opção "Create New Metadata Element" e realizaremos a seguinte configuração:</p>
<ul>
  <li>Hostname: localhost</li>
  <li>Port: a porta utilizada, no caso é 27018 como foi configurada</li>
</ul>
<p>E depois disso, selecionaremos "Get Databases" para que ele se conecte ao banco e busque todos os bancos. Caso esteja tudo certo teremos a seguinte tela.</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/e5888f60-f8c4-4e79-9283-6a0e88c73c1f" alt="img1">
</p>
<p>Vamos selecionar qual banco de dados queremos conectar. Eu escolherei o "vicentin_filial_D".</p>
<p>Em "MongoDB Connection Name", definiremos um nome para salvarmos e selecionarmos facilmente durante a conexão. Usarei o mesmo nome do banco para uma identificação mais fácil. Ficando então dessa forma.</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/48ccc99e-7b7f-4996-bb42-2ee5fd11eb37" alt="img2">
</p>
<p>Agora vamos clicar em "Get Collections" para que ele busque as coleções presentes no Database. Ficando dessa forma.</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/606d03bd-46fa-465f-85f5-a247818265c3" alt="img3">
</p>
<p>Para buscar todos os campos vamos para a opção "Fields" e apos isso em "Get Fields" caso tudo de certo, teremos uma tela parecida com essa.</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/ebd42879-8298-42cb-b2f2-60029db426c6" alt="img4">
</p>
<p>Dessa forma vamos executar a ação e verificar se esta retornando as informações em "View Output".</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/25f6ba2a-03bd-444a-bc81-79a5368a1522" alt="img5">
</p>
<p>Ele está lento um arquivo 'JSON' diretamente do MongoDB, ele está conectado em tempo real com o banco, então caso alguma alteração no banco seja feita, quando for executado a ação ele irá atualizar os valores.</p>
<p>Nesse exemplo, vamos preparar para que ele some a quantidade total de cada produto, por exemplo. Se a gente for olhar no banco, ele está da seguinte forma:</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/93c5fae3-9ec5-4d46-b4bb-d7b39ba6a9a6" alt="img6">
</p>
<p>A ideia é que ele procure, por exemplo, todos os outros 1,000,000 produtos do banco que contêm o nome "Hortelã" e some o campo "quantidade" com os outros do mesmo nome, e assim para os outros produtos. Para que isso aconteça iremos escolher a ação chamada "Select Values"</p>

<h4>Selecionar os valores</h4>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/74feb58e-a817-45ca-819f-54347fa2f9d1" alt="img8">
</p>
<p>Nesse caso, eu quero selecionar apenas o "nome" e "quantidade" para que os outros valores não interfiram ou de algum problema com a consulta, ficando dessa forma</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/13178574-37b2-4b32-a6b5-3ca79d144e3d" alt="img9">
</p>
<p>Apos executar novamente a ação vamos consultar o "View Output" dessa ação</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/de6f553a-27b8-4231-812d-e1e410064c70" alt="img9">
</p>
<p>Aqui ele já está retornando apenas o nome e a quantidade de produtos, lembrando que quando a inserção foi feita, vai ter nome repetidos com quantidades diferentes. No exemplo a baixo podemos ter uma noção, ele está mostrando apenas 100 linhas, no codigo total temos 1,000,000 linhas. Então não levem esses numeros de quantidade em consideração.</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/cb714081-9482-4479-83f5-94674477efea" alt="img10">
</p>
<h4>Ordenar os valores</h4>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/96cf0ab5-f64d-471a-b717-291faf5e107f" alt="img11">
</p>
<p>Nessa ação vamos ordernar as duas colunas, vamos ordernar o nome de A a Z, mas não vamos ordernar a quantidade, para que ele apenas ordene a lista toda pelo nome.</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/b2a87ead-9a92-4af4-b98c-f7aadf59ed00" alt="img12">
</p>
<p>Apos executar novamente a ação vamos consultar o "View Output" dessa ação, lembrado que irá mostrar apenas as 100 primeiras linhas.</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/9954002c-32f5-4b81-8776-914dcaae212e" alt="img13">
</p>
<p>Para dar continuidade vamos utilizar a ultima ação desse projeto, realizando um Group By dessas duas informações. Para isso iremos utilizar a ação "Group By"</p>
<h4>Agrupar os valores</h4>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/e5f7ef7c-d118-43b7-ab20-385b95b38612" alt="img14">
</p>
<p>Agora vamos agrupar pelo nome, realizando uma agregação de quantidade, realizando a soma dos produtos com nomes iguais, ficando dessa forma</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/6dcda093-fb3c-4f30-a609-bfdb9dca96a0" alt="img15">
</p>
<p>Apos executar novamente a ação vamos consultar o "View Output" dessa ação</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/f34c3234-f75c-46b9-bc88-df75a511ab0a" alt="img16">
</p>
<p>So para melhorar a visualização, vamos utilizar mais um "Sort Rows" para aparecer o produto que mais tem quantidade no estoque.</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/49a76f8a-8f5b-42b8-9cdd-62ecc3798498" alt="img17">
</p>
<p>Configurando a coluna "count" que foi criada quando foi feito o "Group By".</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/5d4b4823-ba4b-4033-8708-cd9dab17fc67" alt="img18">
</p>
<p>Então no Database "vicentin_filial_D" e a collection "produtos_estoque_A" o produto "Shampoo" é o que mais tem quantidade.</p>
<p>O pipeline completo com todas as ações ficará dessa forma.</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/2cebedbb-ad52-47cf-a484-ef4dcac38f55" alt="img19">
</p>
<h4>Atualizando a quantidade do produto "Shampoo" e executando o Pipeline novamente.</h4>

```python
db.produtos_estoque_A.find({nome: "Shampoo"})
```
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/0fe96945-41a7-42e9-aa1c-1a26c052f10d" alt="img20">
</p>
<p>Realizando o Update do produto "Shampoo" como existem muitos produtos com o mesmo nome, eu irei usar o ID para alterar apenas a informação de uma informação. Nesse caso o ID 77939316 
o mesmo que está na imagem a cima.</p>

```python
db.produtos_estoque_A.updateOne(
   { id: 77939316 },
   { $set: { quantidade: 100 } }
)
```
<p>Apos consultar novamente, o mesmo ID.</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/a23f5107-8874-43d0-91b1-f1ee3b8e14d2" alt="img21">
</p>
<p>Vamos executar nossa Pipeline agora e verificar como ficou a consulta final, novamente.</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/c24fc306-421a-4787-9566-e9442cb48687" alt="img22">
</p>
<p>Como adicionamos mais 40 Shampoo no campo quantidade ele aparece esses 40 produtos a mais.</p>

<h4>Log de operações</h4>
<p>Todo processo tem um Log das operações, que pode ser verificado caso de algum erro.</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/eb7d522a-41a1-4531-b6d6-31105c1f3913" alt="img23">
</p>
<h2>Concatenando com outro Database</h2>
<h4>Vamos concatenar com a vicentin_filial_E</h4>
<p>Para isso, vamos aproveitar o Pipeline anterior e adicionar uma nova ação chamada "Concat Fields" e utilizar uma outra ação de "MongoDB Input" .</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/e85379d7-61f8-4e43-8a83-742bbe715adb" alt="img24">
</p>
<p>Irei configurar o "MongoDB Input" com as informações do "vicentin_filial_E"</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/28fa3413-7654-49f7-a572-f328649fefce" alt="img25">
</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/277ba7a4-566f-47c9-9073-f0bd37170b35" alt="img25">
</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/85522947-42c3-48ac-9a3f-2d29a2bcbc36" alt="img26">
</p>
<p>Vamos ligar os dois "MongoDB Input" com o "Concat Fields" ficando dessa forma</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/9abe7054-9e8b-4847-95be-122c940b6641" alt="img27">
</p>
<p>A configuração ficará da seguinte forma</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/b524a640-9e29-450a-a690-f6427eb6f504" alt="img28">
</p>
<p>Apenas essas alterações são feitas, o Pipiline ficará assim agora</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/020e1189-b5d0-4f32-9022-2c228f8c513a" alt="img29">
</p>
<p>Apos consultar os dados novamente</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/2f9fb7ec-4dcc-4851-a272-d18e9e730cb6" alt="img30">
</p>
<h4>Log de operações</h4>
<p>O log ficará de seguinte forma agora</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/apache-hop-e-spark/assets/31457038/23cd908e-6285-44a1-8123-2731fe62dde6" alt="img31">
</p>
<p>
<ul>
    <li>A conexão com o MongoDB da filial D tem 1.000.000 de produtos.</li>
    <li>A conexão com o MongoDB da filial E tem 1.000.000 de produtos.</li>
    <li>O processo de concatenação está recebendo esses arquivos e juntando-os em 2.000.000 de registros.</li>
    <li>O processo de ordenação de linhas (SortRows) está agrupando os produtos, resultando em 111 produtos com nomes únicos.</li>
    <li>O processo de ordenação (OrderBy) está retornando esses 111 produtos em ordem decrescente.</li>
</ul>
</p>


