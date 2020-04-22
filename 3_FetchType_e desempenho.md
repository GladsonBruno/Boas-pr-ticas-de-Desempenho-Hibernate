## Problemas de desempenho com FetchType

Um problema comum de desempenho está relacionado ao do **FetchType** errado para determinada entidade, o **FetchType** é especificado no mapeamento da entidade e define quando um relacionamento será carregado. Usar o **FetchType** errado pode resultar em um grande número de consultas que são executadas para carregar as entidades necessárias.

Um problema comum é encontrarmos algo como o exemplo abaixo: 

```java
@OneToMany(fetch = FetchType.EAGER)
```

O **FetchType.EAGER** quando utilizado carrega todos os relacionamentos de uma entidade específica, em entidades que não possuem muitos relacionamentos isso não um grande problema, porém quando a entidade possui um grande número de relacionamentos isso pode se tornar um problema de desempenho na aplicação.

Recomendamos o uso do **FetchType.LAZY** pois o mesmo carrega apenas o necessário da entidade, muitas pessoas não o utilizam para evitar a **LazyInitializationException**, o problema é resolvido de fato, porém a consequência disso é vista diretamente no desempenho da aplicação, para resolver o problema de **LazyInitializationException** utilizando o **FetchType.LAZY** verifique este [artigo]( https://thoughts-on-java.org/lazyinitializationexception/ ).



O principal problema da definição do **FetchType** é que você pode definir apenas um por relacionamento.

Para contornar este problema e ao mesmo tempo não utilizar o **FetchType.EAGER** para realizar as consultas você tem duas opções:

*  Você pode fazer isso como parte de sua instrução JPQL usando FETCH JOIN em vez de JOIN. A palavra-chave FETCH adicional informa ao Hibernate não apenas para unir as duas entidades na consulta, mas também para buscar as entidades relacionadas no banco de dados. Exemplo:

  ```java
  SELECT DISTINCT a FROM Author a JOIN FETCH a.books b
  ```

* A outra opção é utilizar um @NamedEntityGraph, você pode ver neste [artigo]( https://thoughts-on-java.org/jpa-21-entity-graph-part-1-named-entity/ ) como utilizar este recurso do JPA 2.1