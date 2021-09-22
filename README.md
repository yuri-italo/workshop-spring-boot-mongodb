# Projeto MongoDB com Spring Boot

## Objetivo geral: 

- Compreender as principais diferenças entre paradigma orientado a documentos e relacional 
-  Implementar operações de CRUD 
-  Refletir sobre decisões de design para um banco de dados orientado a documentos 
-  Implementar associações entre objetos 
  - Objetos aninhados 
  -  Referências
- Realizar consultas com Spring Data e MongoRepository 

## Entity User e REST funcionando 

 **Checklist para criar entidades:** 

- Atributos básicos 
-  Associações (inicie as coleções) 
-  Construtores (não inclua coleções no construtor com parâmetros) 
-  Getters e setters 
-  hashCode e equals (implementação padrão: somente id) 
-  Serializable (padrão: 1L) 

 **Checklist:** 

- No subpacote domain, criar a classe User 
-  No subpacote resources, criar uma classe UserResource e implementar nela o endpoint GET

## Conectando ao MongoDB com repository e service

![image](https://user-images.githubusercontent.com/81713250/134422300-ed28db96-2076-42f3-9dc5-24e2cf6c96ca.png)

**Referências:**<br>
 https://docs.spring.io/spring-boot/docs/current/reference/html/common-application-properties.html 
 https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-nosql.html 
 https://stackoverflow.com/questions/38921414/mongodb-what-are-the-default-user-and-password 

**Checklist:** 

- Em pom.xml, incluir a dependência do MongoDB: 

&lt;dependency&gt; <br>
 &emsp;&lt;groupId&gt;org.springframework.boot&lt;/groupId&gt; <br>
 &emsp;&lt;artifactId&gt;spring-boot-starter-data-mongodb&lt;/artifactId&gt; <br>
&lt;/dependency&gt;  

- No pacote repository, criar a interface UserRepository 
-  No pacote services, criar a classe UserService com um método findAll 
-  Em User, incluir a anotação @Document e @Id para indicar que se trata de uma coleção do MongoDB 
-  Em UserResource, refatorar o código, usando o UserService para buscar os usuários 
-  Em application.properties, incluir os dados de conexão com a base de dados: 
  spring.data.mongodb.uri=mongodb://localhost:27017/workshop_mongo  
-  Subir o MongoDB (comando mongod) 
-  Usando o MongoDB Compass: 
  - Criar base de dados: workshop_mongo 
  -  Criar coleção: user 
  -  Criar alguns documentos user manualmente 
- Testar o endpoint /users

## Operação de instanciação da base de dados

**Checklist:** 

- No subpacote config, criar uma classe de configuração Instantiation que implemente CommandlLineRunner 
- Dados para copiar: 

User maria = new User(null, "Maria Brown", "maria@gmail.com"); <br>
User alex = new User(null, "Alex Green", "alex@gmail.com"); <br>
User bob = new User(null, "Bob Grey", "bob@gmail.com"); <br>

## Usando padrão DTO para retornar usuários 

**Referências:** 
 https://pt.stackoverflow.com/questions/31362/o-que-e-um-dto

**DTO (Data Transfer Object):** é um objeto que tem o papel de carregar dados das entidades de forma simples, 
podendo inclusive "projetar" apenas alguns dados da entidade original. Vantagens: 

- Otimizar o tráfego (trafegando menos dados) 

- Evitar que dados de interesse exclusivo do sistema fiquem sendo expostos (por exemplo: senhas, dados de 
  auditoria como data de criação e data de atualização do objeto, etc.) 

- Customizar os objetos trafegados conforme a necessidade de cada requisição (por exemplo: para alterar 
  um produto, você precisa dos dados A, B e C; já para listar os produtos, eu preciso dos dados A, B e a 
  categoria de cada produto, etc.). 

**Checklist:** 

- No subpacote dto, criar UserDTO 
- Em UserResource, refatorar o método findAll 

## Obtendo um usuário por id

**Checklist:** 

- No subpacote service.exception, criar ObjectNotFoundException 
-  Em UserService, implementar o método findById 
-  Em UserResource, implementar o método findById (retornar DTO) 
-  No subpacote resources.exception, criar as classes: 
  - StandardError 
  -  ResourceExceptionHandler

## Inserção de usuário com POST 

**Checklist:** 

- Em UserService, implementar os métodos insert e fromDTO 
-  Em UserResource, implementar o método insert 

## Deleção de usuário com DELETE

**Checklist:** 

- Em UserService, implementar o método delete 
-  Em UserResource, implementar o método delete 

## Atualização de usuário com PUT

**Checklist:** 

- Em UserService, implementar os métodos update e updateData 
-  Em UserResource, implementar o método update 

## Criando entity Post com User aninhado

**Nota:** objetos aninhados vs. referências <br>

**Checklist:** 

- Criar classe Post 
- Criar PostRepository 
- Inserir alguns posts na carga inicial da base de dados 

## Projeção dos dados do autor com DTO 

**Checklist:** 

- Criar AuthorDTO 
-  Refatorar Post 
-  Refatorar a carga inicial do banco de dados 
  - **IMPORTANTE:** agora é preciso persistir os objetos User antes de relacionar 

## Referenciando os posts do usuário

**Checklist:** 

- Em User, criar o atributo "posts", usando @DBRef 
  - Sugestão: incluir o parâmetro (lazy = true) 
- Refatorar a carga inicial do banco, incluindo as associações dos posts

## Endpoint para retornar os posts de um usuário

**Checklist:** 

- Em UserResource, criar o método para retornar os posts de um dado usuário

## Obtendo um post por id 

**Checklist:** 

- Criar PostService com o método findById 
-  Criar PostResource com método findById

## Acrescentando comentários aos posts

**Checklist:** 

- Criar CommentDTO 
-  Em Post, incluir uma lista de CommentDTO 
-  Refatorar a carga inicial do banco de dados, incluindo alguns comentários nos posts 

## Consulta simples com query methods

**Referências:** 
 https://docs.spring.io/spring-data/mongodb/docs/current/reference/html/ 
 https://docs.spring.io/spring-data/data-document/docs/current/reference/html/ 

**Consulta:**  

 "Buscar posts contendo um dado string no título" 

**Checklist:** 

- Em PostRepository, criar o método de busca 
-  Em PostService, criar o método de busca 
-  No subpacote resources.util, criar classe utilitária URL com um método para decodificar parâmetro de URL 
-  Em PostResource, implementar o endpoint 

## Consulta simples com @Query

**Referências:** 
 https://docs.spring.io/spring-data/mongodb/docs/current/reference/html/ 
 https://docs.spring.io/spring-data/data-document/docs/current/reference/html/ 
 https://docs.mongodb.com/manual/reference/operator/query/regex/ 

**Consulta:**  

 "Buscar posts contendo um dado string no título" 

**Checklist:** 

- Em PostRepository, fazer a implementação alternativa da consulta 
-  Em PostService, atualizar a chamada da consulta 

## Consulta com vários critérios

**Consulta:**  

"Buscar posts contendo um dado string em qualquer lugar (no título, corpo ou comentários) e em um dado
intervalo de datas" 

**Checklist:** 

- Em PostRepository, criar o método de consulta 
- Em PostService, criar o método de consulta 
- Na classe utilitária URL, criar métodos para tratar datas recebidas 
- Em PostResource, implementar o endpoint 

 

![image](https://user-images.githubusercontent.com/81713250/134422391-99e6d0dc-5c83-4829-b36c-70b97d519965.png)

![image](https://user-images.githubusercontent.com/81713250/134422439-bb0144a1-0404-487c-901f-14a4af9e0958.png)

![image](https://user-images.githubusercontent.com/81713250/134422511-166d28b7-4115-4c6d-a6a9-3da8f73937d9.png)

![image](https://user-images.githubusercontent.com/81713250/134422914-fd23eabd-b03e-45a9-9e14-6e2cf7510667.png)
