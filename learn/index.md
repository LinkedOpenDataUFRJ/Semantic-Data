---
layout: page
---
[Introdução](./index.md#introdu%C3%A7%C3%A3o-1)
---
[Os dados - parte 1](./index.md#os-dados---parte-1-1)
---
[Os dados - parte 2](./index.md#os-dados---parte-2--c%C3%B3digo-rdf)
---
[Aula 3 - SPARQL](./index.md#aula-3---sparql)
---

# Introdução

Esse curso trata do aprendizado da linguagem de busca SPARQL e de seu uso em diversas bases para a obtenção de informação nos moldes desejados. O mesmo é produzido pelos alunos de pesquisa, estudantes de Ciência da Computação, sendo esse um projeto para divulgar o conhecimento obtido para que outros possam fazer proveito tendo o domínio dessa ferramenta. 
  
A linguagem mencionada possibilita a exploração do conteúdo em triplas RDF existente na nuvem de dados triplificados. Tendo em mente que a citada nuvem de dados tem crescido e faz parte de inúmeros projetos internacionalmente e no Brasil envolvendo o processamento e armazenamento de dados com mais metadados e, portanto, com maior clareza e valor informacional sobre o que está armazenado.
  
Nesse breve curso serão apresentados alguns conhecimentos base para compreender o que está por trás das buscas, compreendendo o formato do dado.
  
Inicialmente haverão três partes: Os dados, que trata de explicar como os dados são formatados e citar exemplos concretos para entendimento. A linguagem SPARQL, que introduz a linguagem em si mostrando conceitos desde os mais iniciais e básicos, até alguns mais avançados, permitindo buscas mais elaboradas. Ferramentas, último conteúdo dessa primeira parte, compreende as ferramentas utilizadas para armazenar e executar as buscas a fim de explicitar pontos fortes e fracos e auxiliar em possíveis análises e implementações de cada indivíduo.



Projetos europeus : http://www.smartopendata.eu/partners

Linked Data :http://linkeddata.org/

Transparência em governos : https://conferences.oreilly.com/gov2-international


# Os dados - Parte 1

Nesse parte do conteúdo será tratada a forma na qual os dados trabalhados nas buscas são estruturados e sua natureza. O conhecimento do formato se faz necessário para compreender a estrutura das buscas que serão explicitadas nas partes subsequentes.
As buscas feitas em SPARQL obtém os dados que tem relação com as características exigidas na confecção da mesma. Enquanto em bancos do tipo relacional são retornadas linhas de dados como em planilhas, a linguagem em questão trabalha com os dados triplificados que são diferenciados, principalmente em seu armazenamento.

Uma base de dados triplificados trabalha com endereços web conhecidos como IRI (Internationalized Resource Identifier). Esses identificam cada dado de forma única, ou seja, fazem o papel da chave primária dos dados relacionais. 
Um elemento primordial a ser explicado é o conceito de tripla. Esse define o modo como os dados se apresentam nessas bases em questão. Basicamente, se tratam de um conjunto de sujeito, predicado e objeto. Assim, haverá sempre um elemento em questão, um sujeito, que possuirá um atributo ou dado de sua posse, um objeto, que se conecta ao sujeito possuindo um significado definido, ou seja, um predicado. De tal forma pode-se simplificar pelo seguinte diagrama :



![Tripla](./Os%20dados%20-%20Parte%201_0_0.png)


Como mostrado, um sujeito possui um objeto associado por meio de um predicado. Da mesma forma, um sujeito pode possuir vários objetos relacionados ao mesmo. Como exemplo clarificador pode-se formatar os dados de uma certa pessoa que possui um nome e uma idade, bem como uma profissão por exemplo. A seguir uma demonstração da transformação desse exemplo nesse formato, ou seja, a triplificação:

![Exemplo_Tripla](./Os%20dados%20-%20Parte%201_1_1.png)

Como é possível perceber a estrutura garante uma boa flexibilidade, podendo ao passar do tempo ser expandido o quanto se sabe, tem de informação, a respeito de um ou mais elementos do banco triplificado. Esse formato triplificado é conhecido como RDF(Resource Description Framework) podendo ser codificado em várias linaguagens de programação como XML e Turtle, futuramente mostradas.





Referências:

 SPARQL : https://www.w3.org/TR/rdf-sparql-query/
 
 IRI : https://www.w3.org/TR/rdf11-concepts/#dfn-iri
 
 Tripla : https://www.w3.org/TR/rdf11-concepts/#dfn-rdf-triple
 
 RDF : https://www.w3.org/RDF/
 
Acessado em 13/09/2018


# Os dados - Parte 2 : Código RDF

Nessa segunda parte referente aos dados, serão discutidas as codificações dos dados triplificados, previamente explicados.
  
Como mencionado, os dados triplificados consistem o formato RDF, que determinam a estrutura de grafo. Contudo, esses devem ser codificados em linguagens para serem utilizados e consumidos em máquinas. Os principais formatos são o Turtle e o XML.
  
Primeiramente será abordada a codificação Turtle(Terse RDF Triple Language). Essa linguagem, feita de modo que a leitura por humanos seja de melhor compreensão, é responsável por organizar os dados fazendo os relacionamentos das triplas. Essa linguagem simplesmente representa pela agregação de triplas de URI's os elementos a serem armazenados, sendo referentes a sujeitos, predicados e objetos.

Ex:

```turtle
@prefix ex:http://exemplo.dominio.com/

<ex:sujeito> <ex:predicado> <ex:objeto>.

```

Assim o exemplo mostrado na aula anterior pode ser representado da seguinte forma:

```turtle
@prefix ex:http://exemplo.dominio.com/

<ex:Maria> <ex:idade> 30^^xsd:int.
<ex:Maria> <ex:profissao> <ex:Medica>.

```

Como é possível ver foram representadas as triplas que definem as informações de idade e profissão referentes ao sujeito Maria.

A forma que foi mostrada é bem explícita e fácil de ser compreendida por um ser humano, mas esse formato ainda pode ser mais bem reduzido, aglutinando os elementos referentes a um mesmo sujeito em uma sequência de comandos, com ponto e vírgula em cada nova tripla e um ponto final na última tripla representada. 

Ex:

```turtle
@prefix ex:http://exemplo.dominio.com/

<ex:Maria> <ex:idade> 30^^xsd:int;
           <ex:profissao> <ex:Medica>.

```
Como é possível perceber não foi necessário repetir o sujeito, pois na lista estarão todos os predicados e objetos referentes ao mesmo sujeito, cada um separado com um ponto e vírgula e no fim de todas as triplas desse sujeito um ponto indica o término.


Um elemento de certa forma curioso que surgiu foi o prefixo definido pela palavra prefix logo no início. Como já comentado anteriormente as triplas se baseam em representações com URI's, logo devem ser usados endereços web grandes para cada um dos elementos. Isso geraria muito trabalho e um código pouco organizado, mas, para tal, existe a definição de prefixo, assim quando, como no exemplo, se tem ex:Maria, o endereço desse elemento é o endereço do prefixo mais o nome do elemento, assim o endereço mencionado seria http://exemplo.dominio.com/Maria. Os nomes que vem após o endereço do prefixo como Maria e profissão, são chamados de resources, ou seja, recursos, elementos presentes na base que estão sendo usados.




# Aula 3 - SPARQL

Nesta aula serão exibidos os conteúdos relacionados à linguagem de busca SPARQL. Essa se assemelha a SQL, usada em bancos de dados relacionais. 

A linguagem SPARQL começou a ser estruturada em 2004 com um primeiro rascunho em 12 de outubro. Contudo, a primeira versão estável só surgiu em 2008. Essa foi criada voltada para a nova web, conhecida como 3.0, na qual a semântica é muito mais aplicada e os dados recebem maior valor e carga de significado, semântica. 

A criação da linguagem em questão foi feita com o intuito de explorar a estrutura de triplas RDF para obter não só os dados armazenados como possíveis combinações entre os conteúdos de somente uma ou muitas bases, explorando as mais diversas propriedades dos dados.

As buscas podem ser analisadas por partes: 

1.Variáveis

2.Cláusulas

  - SELECT

  - WHERE

3.Extras

  - FILTER

  - OPTIONAL


## 1.Variáveis

As variáveis apresentam um formato próprio, são iniciadas com uma interrogação e são os elementos não definidos da busca onde representarão as mais diversas URI’s que se encaixem nos parâmetros da busca.

Ex:

```sparql
?sujeito ?predicado ?objeto
```

## 2.Cláusulas:

As cláusulas que definem os pedidos sendo feitos em uma busca, essas que compõem o corpo e o sentido de cada operação desse tipo.

### SELECT

Tal como no modelo relacional, ou seja, em SQL, SELECT define que elementos processados na consulta serão exibidos, ou seja, quais variáveis terão seus valores mostrados.

Ex:

```sparql
SELECT ?sujeito ?objeto
````

### WHERE

Essa cláusula é muito importante, pois nela é que são definidos os padrões de triplas buscados, ou seja, o que realmente está sendo procurado e quais variáveis serão usadas para armazenar cada uma dessas informações.

Ex:
```sparql
SELECT ?sujeito ?objeto
WHERE{
	?sujeito ?predicado ?objeto.
}
````
## Extras

Essa parte é referente a operações mais refinadas que podem ser feitas. Até o instante dados, conteúdo pode ser buscado e obtido, mas com o refinamento será possível facilitar a eliminação de casos não desejados, ou restringir melhor a ação de busca.

### FILTER

Uma função existente em SPARQL que permite realizar outras funções dentro de si para estipular tipos de padrões a serem filtrados, obtidos dos resultados retornados a partir das cláusulas anteriormente mencionadas.

Ex:

```sparql
SELECT ?sujeito ?objeto
WHERE{
	?sujeito ?predicado ?objeto.
	?sujeito ex:nome ?nome.
	FILTER langMatches(lang(?nome),”EN”).
}
````
A busca em questão obtém sujeitos com predicados e seus objetos, mostrando somente os sujeitos e seus objetos. Contudo, somente aparecerão aqueles que tem seu nome registrado no Inglês.



