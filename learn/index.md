---
layout: page
---
[Introdução](https://github.com/LinkedOpenDataUFRJ/Semantic-Data/blob/dev/learn/index.md#introdu%C3%A7%C3%A3o-1)
---
[Os dados - parte 1](https://github.com/LinkedOpenDataUFRJ/Semantic-Data/blob/dev/learn/index.md#os-dados---parte-1-1)
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



![Tripla](https://github.com/LinkedOpenDataUFRJ/Semantic-Data/blob/dev/learn/Os%20dados%20-%20Parte%201_0_0.png)


Como mostrado, um sujeito possui um objeto associado por meio de um predicado. Da mesma forma, um sujeito pode possuir vários objetos relacionados ao mesmo. Como exemplo clarificador pode-se formatar os dados de uma certa pessoa que possui um nome e uma idade, bem como uma profissão por exemplo. A seguir uma demonstração da transformação desse exemplo nesse formato, ou seja, a triplificação:

![Exemplo_Tripla](https://github.com/LinkedOpenDataUFRJ/Semantic-Data/blob/dev/learn/Os%20dados%20-%20Parte%201_1_1.png)

Como é possível perceber a estrutura garante uma boa flexibilidade, podendo ao passar do tempo ser expandido o quanto se sabe, tem de informação, a respeito de um ou mais elementos do banco triplificado. Esse formato triplificado é conhecido como RDF(Resource Description Framework) podendo ser codificado em várias linaguagens de programação como XML e Turtle, futuramente mostradas.





Referências:

 SPARQL : https://www.w3.org/TR/rdf-sparql-query/
 
 IRI : https://www.w3.org/TR/rdf11-concepts/#dfn-iri
 
 Tripla : https://www.w3.org/TR/rdf11-concepts/#dfn-rdf-triple
 
 RDF : https://www.w3.org/RDF/
 
Acessado em 13/09/2018

