<h1 align="center">Acessando arquivos do MongoDB via Apache Hop e Spark</h1>
<p>Nesse projeto iremos aproveitar o projeto <a href="https://github.com/mateusvicentin/cluster-mongodb" target="_blank">Cluster MongoDB</a> iremos conectar em tempo real via Apache Hop e realizar operações dentro do ambiente, e baixar um CSV dos arquivos e realizar operações parecidas como Apache Spark.</p>

<h2>Apache Hop</h2>
<h4>Para conexão no Apache Hop, iremos utilizar 4 ações:</h4>
<ul>
  <li>Conexão com o banco MongoDB</li>
  <li>Selecionar os valores</li>
  <li>Ordenar os valores</li>
  <li>Agrupar os valores</li>
</ul>
