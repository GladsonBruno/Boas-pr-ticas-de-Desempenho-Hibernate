## Problemas de desempenho com FetchType

 Buscar muitos dados é o problema número um que causa problemas de desempenho quando se trata de usar JPA e Hibernate. Isso ocorre porque o JPA facilita a busca de mais dados do que você realmente precisa. 

Este problema normalmente está relacionado ao tipo de **FetchType** utilizado. O **FetchType** é especificado no mapeamento da entidade e define quando um relacionamento será carregado. Usar um **FetchType** inadequado pode resultar em um grande número de consultas que são executadas para carregar as entidades necessárias.

Um problema comum é encontrarmos algo como o exemplo abaixo: 

```java
@OneToMany(fetch = FetchType.EAGER)
```

O **FetchType.EAGER** quando utilizado carrega todos os relacionamentos de uma entidade específica, em entidades que não possuem muitos relacionamentos isso não é um grande problema, porém quando a entidade possui um grande número de relacionamentos isso pode se tornar um grande problema de desempenho na aplicação.

Recomendamos o uso do **FetchType.LAZY** pois o mesmo carrega apenas o necessário da entidade, algumas pessoas não o utilizam para evitar o **LazyInitializationException**, porém a consequência disso é vista diretamente no desempenho da aplicação, para resolver o problema de **LazyInitializationException** utilizando o **FetchType.LAZY** verifique este [artigo]( https://thoughts-on-java.org/lazyinitializationexception/ ).



O principal problema da definição do **FetchType** é que você pode definir apenas um por relacionamento.

Para contornar este problema e ao mesmo tempo **NÃO** utilizar o **FetchType.EAGER** em relacionamentos que realmente precisam carregar os dados de forma ansiosa você tem duas opções:

*  Você pode fazer isso como parte de sua instrução JPQL usando FETCH JOIN em vez de JOIN. A palavra-chave FETCH adicional informa ao Hibernate não apenas para unir as duas entidades na consulta, mas também para buscar as entidades relacionadas no banco de dados. Exemplo:

  ```java
  SELECT DISTINCT a FROM Author a JOIN FETCH a.books b
  ```

* A outra opção é utilizar um **@NamedEntityGraph**, você pode ver neste [artigo]( https://thoughts-on-java.org/jpa-21-entity-graph-part-1-named-entity/ ) como utilizar este recurso do JPA 2.1