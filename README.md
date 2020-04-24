# Resumo das boas práticas de desempenho para Hibernate com JPA

 * Utilização do [Hibernate Statistics](./1_Hibernate_Statistics.md) para coleta de algumas  estatísticas internas sobre cada sessão do Hibernate, ele pode ser útil para ajudar a identificar problemas comuns como [n + 1]( https://vladmihalcea.com/n-plus-1-query-problem/ ) ao analisarmos as estastísicas geradas.
 * Buscar apenas os dados que realmente precisa.

   * Usar paginação de consultas

   * [Evitar **FetchType.EAGER** e usar no lugar dele **FetchType.LAZY**](./3_FetchType_e_desempenho.md).
 * Caso realmente seja necessário usar busca ansiosa usar [EntityGraph]( https://thoughts-on-java.org/jpa-21-entity-graph-part-1-named-entity/ )
 * Evitar o Antipattern [Open Session in View]( https://vladmihalcea.com/the-open-session-in-view-anti-pattern/ ) que consiste em manter a sessão do Hibernate aberta mesmo depois de sair do limite da camada de serviço transacional. Embora isso impeça o lançamento da **LazyInitializationException**, o preço do desempenho é considerável, pois toda inicialização adicional de Proxy não transacional exigirá uma nova conexão com o banco de dados, pressionando o pool de conexões subjacente. 
 * Evitar o uso do parâmetro [**hibernate.enable_lazy_load_no_trans**]( https://vladmihalcea.com/the-hibernate-enable_lazy_load_no_trans-anti-pattern/ ) no **persistence.xml**,  este parâmetro informa ao Hibernate para abrir uma *Sessão* temporária quando nenhuma *Sessão* ativa estiver disponível para inicializar a associação buscada com **FetchType.LAZY**. Isso aumenta o número de conexões de banco de dados usadas, transações de banco de dados e a carga geral no seu banco de dados.  
 * Utilizar [cache de segundo nível](./4_Uso_de_cache_de_segundo_nivel.md) do Hibernate.
 * Utilizar o cache de consulta
 * Utilizar consultas somente leitura.
 * Agrupar em lotes instruções INSERT/UPDATE que devem ser executadas em massa.
 * Deixar o banco de dados manipular operações pesadas de dados utilizando **Store Procedures** e as chamando na aplicação através de **@NamedStoredProcedureQuery**.
 * [Utilizar **java.util.Set** no lugar de **java.util.List** em relacionamentos de muitos para muitos](2_Melhor_maneira_para_uso_da_anotacao_ManyToMany_com_JPA.md).



## Artigos úteis

* [Como encontrar problemas de desempenho no Hibernate]( https://stackify.com/find-hibernate-performance-issues/ )

* [Ajuste de desempenho em aplicações Spring/Hibernate]( https://www.javacodegeeks.com/2014/06/performance-tuning-of-springhibernate-applications.html )

* [10 erros comuns que prejudicam o desempenho no Hibernate]( https://thoughts-on-java.org/common-hibernate-mistakes-cripple-performance/ )

  



