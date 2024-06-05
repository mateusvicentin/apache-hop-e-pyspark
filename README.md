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







