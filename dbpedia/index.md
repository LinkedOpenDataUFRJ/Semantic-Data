---
layout: page
---
# DBpedia

Uma das mais famosas bases públicas de Linked Data existentes na atualidade é a DBpedia. Essa apresenta dados triplificados a partir da infobox da Wikipedia, trazendo algumas informações da renomada enciclopédia online.
A tarefa a seguir visa realizar uma breve exploração e introdução a essa base. Perceber sua importância como hub da nuvem LOD e tomar conhecimento das diferenças de esquema existentes nessas bases públicas triplificadas.

Primeiramente, é de suma importância a ontologia disponibilizada pela DBpedia, que permite modelar e triplificar os mais diversos dados existentes. A seguir, uma breve pesquisa para obter os tipos existentes no esquema:

```sparql
Select DISTINCT ?type
Where {
	?uri a ?type.
}
```


Para ser mais preciso e perceber a variedade de categorias (classes e subclasses) aplicadas somente aos seres humanos, ou seja, às pessoas:
```sparql
Select DISTINCT type
Where {
	?uri a foaf:Person.
?uri a ?type.
}
```

Bem como os mais diversos tipos de propriedades aplicadas a uma pessoa :

```sparql
Select DISTINCT ?property
Where {
	?uri a foaf:Person.
	?uri ?property ?object.
}
```

Um dos problemas que pode surgir em uma base é a variação de termos usados na modelagem. Para exemplificar usaremos um caso simples. Vamos tentar obter os políticos brasileiros (suas IRIs) existentes na base :

```sparql
Select *
Where {
	?uri a foaf:Person.
	?uri dbo:profession dbr:Politician.
	?uri dbo:nationality dbr:Brazil.
}
```

Como foi possível perceber os resultados obtidos não correspondem muito com a realidade, deixando a desejar, aparentando que a base não possui muitos dados. Contudo, vamos executar a seguinte consulta :

```sparql
Select *
Where {
	?uri a foaf:Person.
	?uri dbo:occupation dbr:Politician.
	?uri dbo:nationality dbr:Brazil.
}
```

Os resultados obtidos foram os mesmos? 
Os termos profession e occupation são equivalentes, mas não há expressada clara equivalência dentro da base, o que dificulta o processo de obtenção desses dados.
Infelizmente esse não é o único caso. A seguir uma nova consulta visando retornar os resultados desejados políticos brasileiros, porém explorando outras propriedades.

```sparql
Select DISTICT ?uri
Where {
	?uri a foaf:Person.
	?uri dbo:profession dbr:Politician.
	?uri dbo:birthPlace ?place.
	?place dbo:country dbr:Brazil.
}
```

Ao atentar a alguns casos, será possível perceber que algumas dessas pessoas não possuíam nacionalidade expressa claramente (dbo:nationality), somente possuindo cidade ou, simplesmente, o local de nascimento como o povoado. Assim, pode-se tentar obter o dado de país a partir da propriedade dbo:country associada local de nascimento.
 
Embora esses exemplos possam exibir falhas ou dificuldades em obter dados de bases como essa, os mesmos exibem, na verdade, um reflexo da realidade. Os dados e registros existentes sobre conhecimentos e fatos, principalmente os históricos, não apresentam um formato perfeito e constante e, a capacidade dessas bases de serem flexíveis, diferente das relacionais, permite que com o passar do tempo e, a medida que se descobre mais sobre algo, que esses novos conteúdos possam ser anexados àquela entidade já existente na base.
Um outro fator que permite visualizar um modo de contornar esses problemas pode ser visto na busca a seguir:
 
```sparql
SELECT DISTINCT ?politico ?nlabel
WHERE{
{
?politico ?p dbr:Politician. filter(?p = dbo:profession || ?p = dbo:occupation)
?politico dbo:nationality dbr:Brazil.
?politico rdfs:label ?labeldb.
}
union
{

?politico rdf:type yago:WikicatBrazilianPoliticians.
?politico rdfs:label ?labeldb.

}
union{
?politico ?p dbr:Politician. FILTER(?p = dbo:profession || ?p = dbo:occupation).
?politico dbo:birthPlace ?bp.
?bp dbo:country dbr:Brazil.
?politico rdfs:label ?labeldb.
}
FILTER langMatches(lang(?labeldb),"pt").
BIND (STR(?labeldb)AS ?nlabel).
}
```

Realizando ligações com outros vocabulários (além da própria ontologia da DBpedia), como na consulta acima, onde foi referenciada uma classe (yago:WikicatBrazilianPoliticians) utilizada para determinar explicitamente políticos brasileiros. 
Outro exemplo semelhante se dá por meio da propriedade owl:sameAs. Essa permite que, mesmo tendo sido definida uma entidade de forma própria em uma base que exista a possibilidade de explicitar que uma entrada em uma base é equivalente a outra, em uma base distinta (isso vale também para mapeamento de termos de vocabulários distintos). 

```sparql
SELECT ?politico ?nlabel ?refer
WHERE{
{
?politico ?p dbr:Politician. filter(?p = dbo:profession || ?p = dbo:occupation)
?politico dbo:nationality dbr:Brazil.
?politico rdfs:label ?labeldb.
?politico owl:sameAs ?refer.
}
union
{

?politico rdf:type yago:WikicatBrazilianPoliticians.
?politico rdfs:label ?labeldb.
?politico owl:sameAs ?refer.

}
union{
?politico ?p dbr:Politician. FILTER(?p = dbo:profession || ?p = dbo:occupation).
?politico dbo:birthPlace ?bp.
?bp dbo:country dbr:Brazil.
?politico rdfs:label ?labeldb.
?politico owl:sameAs ?refer.
}
FILTER langMatches(lang(?labeldb),"pt").
BIND (STR(?labeldb)AS ?nlabel).
}
```

Isso permitiria que se junte os dados referentes, por exemplo a uma pessoa, existentes em uma base, com os dados restantes presentes em outra base. A linguagem SPARQL permite esses cruzamentos de forma bem simples. Como exemplo tem-se a busca a seguir que cruza os dados da DBPEDIA e da WIKIDATA comparando as strings dos nomes e, caso exista o dado cadastrado, exibir o sucessor do político encontrado.

```sparql

PREFIX dbo: <http://dbpedia.org/ontology/>
PREFIX dbr: <http://dbpedia.org/resource/>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX foaf: <http://xmlns.com/foaf/0.1/>
PREFIX yago: <http://yago-knowledge.org/resource/>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX wdt: <http://www.wikidata.org/prop/direct/>

select distinct ?politico ?labeldb ?name ?label ?sucessor ?sucessorNome where { 
    service<http://dbpedia.org/sparql>{
		SELECT *
WHERE{
{
?politico ?p dbr:Politician. filter(?p = dbo:profession || ?p = dbo:occupation)
?politico dbo:nationality dbr:Brazil.
?politico rdfs:label ?labeldb.
optional{?politico dbo:successor ?sucessor.
                    ?sucessor rdfs:label ?sucessorNome.
        FILTER langMatches(lang(?sucessorNome),"pt").}
}
union
{

?politico rdf:type yago:WikicatBrazilianPoliticians.
?politico rdfs:label ?labeldb.
optional{?politico dbo:successor ?sucessor.
                    ?sucessor rdfs:label ?sucessorNome.
        FILTER langMatches(lang(?sucessorNome),"pt").}
                     
}
union{
?politico ?p dbr:Politician. FILTER(?p = dbo:profession || ?p = dbo:occupation).
?politico dbo:birthPlace ?bp.
?bp dbo:country dbr:Brazil.
?politico rdfs:label ?labeldb.
optional{?politico dbo:successor ?sucessor.
                    ?sucessor rdfs:label ?sucessorNome.
        FILTER langMatches(lang(?sucessorNome),"pt").}
      }
FILTER langMatches(lang(?labeldb),"pt").
BIND (STR(?labeldb)AS ?nlabel).
}

    }
    
    service<https://query.wikidata.org/sparql>{
        SELECT DISTINCT ?name (sample(?strLabel)as ?label) WHERE {
  			?name wdt:P106 wd:Q82955.
  			?name wdt:P27 wd:Q155.
  			?name rdfs:label ?nameLabel.
  			filter langMatches(lang(?nameLabel),"PT")
            bind (str(?nameLabel) as ?strLabel)
		}group by ?name

    }
  filter (?label=?nlabel)
}limit 100
```
