# Resumo das boas práticas de desempenho para Hibernate com JPA

 * Usar o Hibernate statistics para monitorar o número de querys geradas, pode ser útil para identificar se um processo está sofrendo do problema [n + 1]( https://vladmihalcea.com/n-plus-1-query-problem/ )
 * Buscar apenas os dados que realmente precisa.
 * Usar paginação de consultas
 * Evitar FetchType.EAGER e usar no lugar dele FetchType.LAZY.
 * Caso realmente seja necessário usar busca ansiosa usar [EntityGraph]( https://thoughts-on-java.org/jpa-21-entity-graph-part-1-named-entity/ )
 * Evitar o Antipattern [Open Session in View]( https://vladmihalcea.com/the-open-session-in-view-anti-pattern/ ) que consiste em manter a sessão do Hibernate aberta mesmo depois de sair do limite da camada de serviço transacional. Embora isso impeça o lançamento da **LazyInitializationException**, o preço do desempenho é considerável, pois toda inicialização adicional de Proxy não transacional exigirá uma nova conexão com o banco de dados, pressionando o pool de conexões subjacente. 

 * Evitar o uso do parâmetro [**hibernate.enable_lazy_load_no_trans**]( https://vladmihalcea.com/the-hibernate-enable_lazy_load_no_trans-anti-pattern/ ) no **persistence.xml**,  este parâmetro informa ao Hibernate para abrir uma *Sessão* temporária quando nenhuma *Sessão* ativa estiver disponível para inicializar a associação buscada com **FetchType.LAZY**. Isso aumenta o número de conexões de banco de dados usadas, transações de banco de dados e a carga geral no seu banco de dados.  
 * Utilizar [cache de segundo nível](./4_Uso_de_cache_de_segundo_nivel.md) para que seja feito o compartilhamento de cache entre sessões.
 * Utilizar o cache de consulta
 * Utilizar consultas somente leitura.
 * Deixar o banco de dados manipular operações pesadas de dados utilizando Store Procedures e as chamando na aplicação através de @NamedStoredProcedureQuery