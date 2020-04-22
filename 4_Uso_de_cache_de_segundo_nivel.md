## Uso de cache para evitar a leitura de dados repetidos

O design modular do aplicativo e as sessões paralelas do usuário geralmente resultam na leitura de dados repetidos várias vezes. É bastante óbvio que isso é uma sobrecarga que você deve tentar evitar. 

Uma maneira de fazer isso é armazenar em cache os dados que eles costumam ler e que não são alterados com frequência.

### Cache de segundo nível

 O **cache de segundo nível** é independente da sessão e está vinculado ao ciclo de vida do **SessionFactory** , portanto, é destruído apenas quando o **SessionFactory** é fechado (normalmente quando o aplicativo está sendo desligado) , o mesmo também armazena entidades, mas precisa ser ativado configurando algumas propriedades.

Para ativar o cache de segundo nível você ira precisar realizar as seguintes etapas:

1º - Configurar a seguinte propriedade no arquivo **persistence.xml**:

```xml
<property name="hibernate.cache.use_second_level_cache" value="true"/>
```

Esta propriedade diz ao Hibernate para habilitar o uso do cache de segundo nível.



2º - Configurar a seguinte propriedade no **persistence.xml**:

```xml
<property name="hibernate.cache.region.factory_class" value="org.hibernate.cache.ehcache.EhCacheRegionFactory"/>
```

Esta propriedade diz ao Hibernate para utilizar o provider **EHCache** para armazenar o cache de segundo nível.

Para utilizar o provider**EHCache** precisamos definir a seguinte dependência em nosso **pom.xml**:

```xml
<dependency>
   <groupId>org.hibernate</groupId>
   <artifactId>hibernate-ehcache</artifactId>
   <version>5.2.12.Final</version>
</dependency>
```



3º - Configurar o seguinte parâmetro no persistence.xml:

```xml
<shared-cache-mode>ENABLE_SELECTIVE</shared-cache-mode>
```

Este parâmetro diz ao Hibernate para habilitar o compartilhamento de cache apenas para as entidades que possuírem a anotação **@Cacheable** do pacote **javax.persistence.Cacheable** no escopo de classe conforme o exemplo abaixo:

```java
@Entity
@Cacheable
public class Autor {

    // Atributos da classe
    
}
```



4º - Anotar as entidades que serão armazenadas no cache de segundo nível com a anotação **@Cacheable** do pacote **javax.persistence.Cacheable** no escopo de classe.

Um exemplo disso foi dado no passo anterior.