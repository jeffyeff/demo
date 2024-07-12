# demo

https://gist.github.com/jeffyeff/9f472c4d7234b091c5e0411bc7b3d00b


# 1.1


### why spring boot?
- mature ecosystem - more than 2 decades, community, documentation
- loose coupling - DI allows loose coupling, easy testing
- provides default auto configuration
- microservices architecture - suited for it
- layers of application development
- xml and annotation configurations
- spring offers middleware services

### tomcat, servlet
The server to run the java code - tomcat
The java code that runs on this server - servlet


springboot = spring + tomcat - config



### creating project
spring intializr: `start.spring.io`

src/main/java 
- where the java code will be wriiten

src/main/resources
- use static and other resources
- also has the application.properties

### IOC
Inversion of Control (IoC) shifts control(from user to spring) over object creation and flow from application code to a framework or container, enhancing modularity and flexibility. 


### applicationContext.xml
- is kept inside `src/main/resources` so path will be `src/main/resources/applicationContext.xml`
- put the initial schema from the internet into the context file

### General flow
- create interface, create classes
- create bean for class
  `<bean id="<unique-id-name>" class="<qualified-name>" ></bean>`
- using the beans
```java
ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");  

Table table = (Table) context.getBean("<bean-id>");
```
where the bean-id will be of a particular specific class
`Table` is the interface, and we are getting a particular class using `.getbean()`














# 1.2

### DI (dependency injection)
- constructor injection
- setter injection
- field injection (only using annotation)


1. constructor injection (using xml)

```xml
`<bean id="dependencyBean" class="com.example.DependencyClass">     <constructor-arg name="arg1" value="value1"/>     
<constructor-arg name="arg2" ref="anotherBean"/> 
</bean>`
```

`<constructor-arg>`

2. setter injection (using xml)

```xml
`<bean id="dependencyBean" class="com.example.DependencyClass">     <property name="property1" value="value1"/>     
<property name="property2" ref="anotherBean"/> 
</bean>`
```


`<property>`

where arg1 can be some normal class not an object therefore we are giving the value directly using `value=""` 

arg2 is another man-made custom object therefore we have to give reference using `ref=""` attribute. 



# 2.1

# Scope

singleton: always the same bean instance is given
prototype: each time different bean instance is given

you can check by printing the reference id's of the objects from the beans
There are other scopes too, these two are the major ones
(by default it is singleton)

```xml
<bean id="simplePost" class="com.something.SimplePost" scope="prototype"> </bean>
<bean id="simplePostList" class="com.something.SimplePostList" scope="singleton"> </bean>
```

`scope`

# Bean Lifecycle

1. init-method
2. destroy-method

| init-method                                                | destroy-method                                          |
| ---------------------------------------------------------- | ------------------------------------------------------- |
| 1. post construct                                          | 1. pre destroy                                          |
| 2. after the construction of the bean the method is called | 2. before the deletion of the bean the method is called |
| 3. singleton supported                                     | 3. singleton supported                                  |
| 4. prototype supported                                     | 4. no<br>wont give errors, but doesnt work              |
|                                                            |                                                         |


```xml
<bean id="simplePostList" class="com.codingNinjas.SocialMedia.SimplePostList" 
scope="singleton"  
init-method="init"  
destroy-method="destroy"  
></bean>  

<bean id="simplePost" 
class="com.codingNinjas.SocialMedia.SimplePost" 
scope="prototype"  
destroy-method="destroy"  
></bean>
```
`init-method`
`destroy-method`


`init` and `destroy` are method names

scanner and context are required to be closed
![[Pasted image 20240303152640.png]]


# 2.2 


# Annotations

### IOC

`@Component`
or 
`@Component("<unique-name>")`

component annotation is put over classes not interfaces

`@Component` has more semantic similar annotations
- `@Service`
- `@Controller`
- `@Repository`


### DI

`@Autowired`
or
```
@Autowired
@Qualifier("<id-name>")   
```

### Scope

`@Scope("singleton")`

`@Scope("prototype")`

### Bean lifecycle

`@PostConstruct`

`@PreDestroy`



---

```java
InterfaceName obj = context.getBean(InterfaceName.class);
// works only when it has uniquely matched bean

InterfaceName obj = context.getBean("some_id", InterfaceName.class);
// goes to the @Component("some_id") class which implements InterfaceName
```

---
### AnnotationConfigApplicationContext

`ClassPathXMLApplicationContext` and `AnnotationConfigApplicationContext` are the implementation classes of `ApplicationContext` interface
(extra info: therefore you can also use `ApplicationContext` to hold the variable)

```
AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext("com.codingNinjas.codingNinjasApp");
```
here you have to give the package name as String
here you will have to close context manually
or

```
ApplicationContext context = SpringApplication.run(CodingNinjasAppApplication.class, args);
```
here the context will be closed by spring, you don't have to do it


### Spring boot Annotation

`@SpringBootApplication`  internally has (major ones listed)
- `@Configuration` : current and below directories, searches for beans
- `@EnableAutoConfiguration`: 
- `@ComponentScan`


### Configuring info like DBs

configuring info like DB, port nos. is done in `application.properties` file
or 
we can use `application.yml` file
(yaml files are key value pairs)

`server.port=8081`



# 3.1

be careful while adding dependencies, the groupId is important
make sure you are adding the right one same as mentioned in pdf


# 3.2




# 4.1

### Mappings

`@RestController` --> is put over the class
`@RequestMapping("/common_path")` --> is put over the class 


`@GetMapping("/get")`  --> is put over the method
- to get the id, from the url `/get/id/{id}`, use `@PathVariable`

`@PostMapping` --> is put over the method
- to get the request body in method argument `@RequestBody`



# 4.2


### Mappings

`@PutMapping` --> is put over the method
- to get the id, use `@PathVariable`
- to get the request body (contains the update req. object) , use `@RequestBody` in the method argument

`@DeleteMapping` --> is put over the method
- to get the id, from the url `/remove/id/{id}` use `@PathVariable`


### Exception handling

`@ResponseStatus(HttpStatus.NOT_FOUND)` --> is put over the exception class



### Validations


first need to add dependencies in `pom.xml`
1. spring-boot-starter-validation
2. javax-validation

1. spring-boot-start-validation makes sure all the validations from javax-validations are binded and are used properly
2. javax-validation contains the `@Size, @Min...` and other annotations


#### Dependencies to be added code
```xml
<!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-validation -->  
<dependency>  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-starter-validation</artifactId>  
    <version>3.2.3</version>  
</dependency>  
  
  
<!-- https://mvnrepository.com/artifact/javax.validation/validation-api -->  
<dependency>  
    <groupId>javax.validation</groupId>  
    <artifactId>validation-api</artifactId>  
    <version>2.0.1.Final</version>  
</dependency>
```

**NOTE:** after writing these in pom.xml then later reload if it shows red in intellij


from jakarta during imports

`@Valid`  --> put in method argument
- `@Size(min=12, max=234)` --> put over fields/variables (could be Strings, Arrays, Collections)
- `@Min(1) or @Min(value=1)`  ---> put over fields/variables 
- `@Min(1) or @Min(value=1)`  ---> put over fields/variables 

Both `@Min` and `@Max`can be used over numeric fields (int, long, float, double)

so when the validation fails, how do we catch them? `@BindingResult` 

```java
public void somePostMethod(@Valid @RequestBody Hotel hotel, @BindingResult BindingResult bindingResult) {
	if(bindingResult.hasErrors()) {
		throw new SomeException();
	}
}
```



Method ***Argument/Parameter*** annotations
These annotations are put in method arguments/parameters
- `@RequestBody`
- `@PathVariable`
- `@Valid` ``





# 5.1

project


# 5.2
suppose you want your service fetching some info from other service, so you will need to know how to do it in java
Here we have REST template to do that

few methods of RESTtemplate
- `getForObject` - get 
- `getForEntity` - get
- `postForObject` - post
- `exchange` - get, post, put, delete... supported

(you can also look the parameters of the method using the IDE)

So to use these, first you will require the `RestTemplate` 
you cant autowire it directly (in newer versions)
we use `RestTemplateBuilder` 

```java
private RestTemplate restTemplate;

@Autowire
public SomeClass(RestTemplateBuilder restTemplateBuilder) {
	this.restTemplate = restTemplateBuilder;
}
```


And to use 

###  `getForEntity`
you don't get the response body directly

```java
public Long getRating(String id) {
	String url = "http://localhost:8081/rating/";
	ResponseEntity<Long> response = restTemplate.getForEntity(url + id, Long.class);
	return response.getBody();
}

/* public Long getRating(String id) {
	String url = "http://localhost:8081/rating/";
	ResponseEntity<RESPONSE_TYPE> response = restTemplate.getForEntity(url + id, RESPONSE_TYPE.class);
	return response.getBody();
} 
*/
```


### `getForObject`
you get the object directly

```java
public Long getRating(String id) {
	String url = "http://localhost:8081/rating/";
	Long response = restTemplate.getForObject(url + id, Long.class);
	return response;
}
```


### `postForObject`

```java
public void addRating(Map<String, Long> ratingsMap) {
	String url="http://localhost:8081/rating/add";
	restTemplate.postForObject(url, ratingsMap, Object.class)
}
```

`Object.class` because we don't care about the response type. 


### `exchange`

parameters for exchange method
- URL
- Request method
- Http Entity wrapping the body to be sent (request entity)
- Response type

look in pdf notes how to do it



### Handling Exceptions

- ClientErrorException instead of RuntimeErrorException is extended 
- use try and catch

### Message Converters

read from pdf


---

you can use `JsonNode` to store the response, if just to see it

or you will have to use `@JsonProperty("")` and have a custom Java object


for api: https://catfact.ninja/fact
```java
public class Xobj {  
  
    @JsonProperty("fact")  
    public String fact;  
  
    @JsonProperty("length")  
    public String length;  
  
    @Override  
    public String toString() {  
        return "Xobj{" +  
                "fact='" + fact + '\'' +  
                ", length='" + length + '\'' +  
                '}';  
    }  
}
```



# 6.1

### what is the problem if we use just List or Hashmaps to store the data?
Each time we restart the application, the whole earlier data is lost.

### JDBC

Java Database Connectivity
1. makes connection
2. performs operations (save, getdetails)
3. return response

![[Pasted image 20240305164447.png]]

ORM: Object Relational Mapping (maps Objects to Relational DB and vice versa)


### Dependencies
1. mysql connector dependency  (this is for mysql, different for different DBs)
   `mysql-connector-java` , groupId mysql
2. Hibernate annotations
   `spring-boot-starter-data-jpa`

### Annotations for  Model/Entity object

##### `@Entity`  ---> put above the class
it is from javax.persistance package
Specifies that the class is an entity.


##### `@Table(name="")`   ---> put above the class
Tells to create the table and gives name to it. 

If no `Table` annotation is specified for an entity class, the default values apply.

    Example:

    @Entity
    @Table(name="CUST", schema="RECORDS")


##### `@Column` ---> put above the field
Specifies the mapped column for a persistent property or field. 


If no `Column` annotation is specified, the default values apply.

>     Example 1:
> 
>     @Column(name="DESC", nullable=false, length=512)


##### `@Id`
Specifies the primary key of an entity. 


##### `@GeneratedValue(strategy=GenerationType.AUTO)`


##### NOTE
Generate getters and setters and constructors (optional) so that the entity field columns can be set by hibernate






### Concept of transaction
(also more in Arpit Bhayani's system design)

##### Atomic operation: 
1. either happens completely 
2. complete chunk is cancelled and rolled back to previous state

##### Session: 
An object that maintains the connection between a java object application and a database
It provides methods for inserting, updating, and deleting persistent objects and querying database.

1. open session
2. start transaction
3. operation
4. closing the session

To tell Hibernate to start transactions 
`@Transactional`  ---> put over the method in the Service

![[Pasted image 20240305171608.png]]
DAL: Data Access Layer

DAL/DAO/Repository
`@Respository` above the DAL layer 



Entity Manager is a third party API which manages sessions for us
We use it in DAL layer generally to open sessions and make operations

```java
@Autowired
EntityManager entityManager;


Session session = entityManager.unwrap(Session.class);

```

### connecting DB to the code

application.properties file 
or 
application.yml file


sample file, you can always google the file for your database
```
spring:

	datasource:

	url: jdbc:mysql://localhost:3306/CNPayment?createDatabaseIfNotExist=true
	username:
	password:

jpa:
	properties.hibernate-dialect:
	hibernate.ddl-auto: update
```

or using `application.properties` file

```
spring.datasource.url=jdbc:postgresql://localhost:5432/your_database
spring.datasource.username=your_username
spring.datasource.password=your_password

# Optional: Specify a custom driver class name
spring.datasource.driver-class-name=org.postgresql.Driver

# JPA/Hibernate properties (optional but recommended for JPA/Hibernate usage)
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true

```


### Extra info:

When we want some class object in application file.
We don't autowire them. 
We use applicationContext and use `.getBean`(we have seen similar before)
```
Application context = SpringApplication.run(CnPaymentApplication.class, args);

InterfaceOrClassName obj = context.getBean(ClassName.class);
```



### Flow

![Pasted image 20240305172824](https://github.com/user-attachments/assets/c1b733ee-07e7-40c5-8916-7e88ff8f2399)

##### Q. Why interface in between, why not direct class implementation?
So that if tomorrow the DB changes and DAL changes then the required CRUD operations in the interface do not get skipped by the developer
As it would be mandatory to implement the methods of the interface. Therefore we keep a interface in between and mention CRUD operations in that


### Using session for CRUD

also you can look into the session class using IDE
gives more idea about arguments and return types

(Session from hibernate package while importing)

```java

Session session = entityManager.unwrap(Session.class);

Item item = session.get(Item.class, id); // get

session.save(item);          // post
session.update(session);

session.update(item);         // put
//session.update(Object)

session.delete(item)          // delete
//session.delete(Object);
```




# 6.2 Hibernate Relations

https://www.youtube.com/watch?v=H__ELR1y3FQ
https://www.baeldung.com/jpa-hibernate-associations 
https://github.com/sujay000/hibernateRelations/tree/base (my github repo)


Type of relations
1. `@OneToOne`
2. `@OneToMany`
3. `@ManyToMany`

There are two types even in these,
1. uni-directional
2. bi-directional


- @JoinColumn specifies a column for joining an entity association or element collection. 
	- It used to give the foreign key column
	- it adds a column in our table. 
- `mappedBy` is in the entity, which is the non-owning side of the relationship. 
	- whichever class used this, then doesnt have the column in the db.
	- therefore the class which uses `mappedBy` does not have to give this field, while storing this class object in db.

NOTE: sometimes the logic or the way @JoinColumn may feel counter-intuitive. But it is, the way it is.
ex: one-many unidirectional code (https://www.bezkoder.com/jpa-one-to-many-unidirectional/)


##### when it is one to many bi-directional

the @JoinColumn is present in the manyClass because if it present in oneClass, how will oneClass table will keep the ids of manyClass, it would be a list inside the row which is not possible in relational  DB. 



#### Uni-directional, one-to-one mapping

```java
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String username;

    // Unidirectional one-to-one relationship
    @OneToOne(cascade = CascadeType.ALL)
    @JoinColumn(name = "profile_id", referencedColumnName = "id")
    private UserProfile profile;

    // Standard getters and setters
}

@Entity
public class UserProfile {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String phone;
    private String address;

    // No reference to User entity

    // Standard getters and setters
}

```

Dummy data 
`User` table

```sql
+----+----------+------------+
| id | username | profile_id |
+----+----------+------------+
|  1 | john_doe |          1 |
|  2 | jane_doe |          2 |
+----+----------+------------+
```

`UserProfile` table

```sql
+----+-----------+----------------+
| id | phone     | address        |
+----+-----------+----------------+
|  1 | 555-1234  | 123 Main St    |
|  2 | 555-5678  | 456 Elm St     |
+----+-----------+----------------+
```

#### Bidirectional One-to-One Relationship

```java
@Entity
public class Customer {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    // Bidirectional one-to-one relationship
    @OneToOne(cascade = CascadeType.ALL, mappedBy = "customer")
    private CustomerDetail detail;

    // Standard getters and setters
}

@Entity
public class CustomerDetail {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String email;
    private String phone;

    // Reference back to Customer entity
    @OneToOne
    @JoinColumn(name = "customer_id", referencedColumnName = "id")
    private Customer customer;

    // Standard getters and setters
}
```

Dummy Data

`Customer` table

```sql
+----+-----------+
| id | name      |
+----+-----------+
|  1 | Alice     |
|  2 | Bob       |
+----+-----------+
```

`CustomerDetail` table:

```sql
+----+-----------------+-----------+-------------+
| id | email           | phone     | customer_id |
+----+-----------------+-----------+-------------+
|  1 | alice@email.com | 555-6789  |           1 |
|  2 | bob@email.com   | 555-9876  |           2 |
+----+-----------------+-----------+-------------+
```


#### Unidirectional One-to-Many Relationship:

```java
@Entity
public class Library {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    // Unidirectional one-to-many relationship
    @OneToMany(cascade = CascadeType.ALL, orphanRemoval = true)
    @JoinColumn(name = "library_id") // Foreign key in Book table
    private List<Book> books = new ArrayList<>();

    // Standard getters and setters
}

@Entity
public class Book {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String title;

    // No reference to Library entity

    // Standard getters and setters
}
```

`Library` table:
```sql
+----+------------------+
| id | name             |
+----+------------------+
|  1 | Central Library  |
|  2 | Downtown Library |
+----+------------------+
```


`Book` table:
```sql
+----+---------------------+-------------+
| id | title               | library_id  |
+----+---------------------+-------------+
|  1 | The Great Gatsby    |           1 |
|  2 | 1984                |           1 |
|  3 | To Kill a Mockingbird |         2 |
+----+---------------------+-------------+
```

#### Bidirectional One-to-Many Relationship:

```java
@Entity
public class Department {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    // Bidirectional one-to-many relationship
    @OneToMany(mappedBy = "department", cascade = CascadeType.ALL, orphanRemoval = true)
    private List<Employee> employees = new ArrayList<>();

    // Standard getters and setters
}

@Entity
public class Employee {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    // Reference back to Department entity
    @ManyToOne
    @JoinColumn(name = "department_id")
    private Department department;

    // Standard getters and setters
}
```


`Department` table:
```sql
+----+-----------------+
| id | name            |
+----+-----------------+
|  1 | Human Resources |
|  2 | Marketing       |
+----+-----------------+
```


`Employee` table:
```sql
+----+----------------+---------------+
| id | name           | department_id |
+----+----------------+---------------+
|  1 | John Smith     |             1 |
|  2 | Jane Doe       |             1 |
|  3 | Mike Johnson   |             2 |
+----+----------------+---------------+
```








#### Unidirectional Many-to-Many Relationship:

```java
@Entity
public class Student {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    // Unidirectional many-to-many relationship
    @ManyToMany(cascade = CascadeType.ALL)
    @JoinTable(
        name = "student_course",
        joinColumns = @JoinColumn(name = "student_id"),
        inverseJoinColumns = @JoinColumn(name = "course_id")
    )
    private List<Course> courses = new ArrayList<>();

    // Standard getters and setters
}

@Entity
public class Course {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String title;

    // No reference to Student entity

    // Standard getters and setters
}
```

`Student` table:

```sql
+----+------------+
| id | name       |
+----+------------+
|  1 | Alice      |
|  2 | Bob        |
+----+------------+
```

`Course` table:
```sql
+----+-----------------+
| id | title           |
+----+-----------------+
|  1 | Math 101        |
|  2 | English 101     |
+----+-----------------+
```

`student_course` join table:

```sql
+------------+-----------+
| student_id | course_id |
+------------+-----------+
|          1 |         1 |
|          1 |         2 |
|          2 |         1 |
+------------+-----------+
```


#### Bidirectional Many-to-Many Relationship:
```java
@Entity
public class Author {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    // Bidirectional many-to-many relationship
    @ManyToMany(mappedBy = "authors")
    private List<Book> books = new ArrayList<>();

    // Standard getters and setters
}

@Entity
public class Book {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String title;

    // Reference back to Author entity
    @ManyToMany
    @JoinTable(
        name = "author_book",
        joinColumns = @JoinColumn(name = "book_id"),
        inverseJoinColumns = @JoinColumn(name = "author_id")
    )
    private List<Author> authors = new ArrayList<>();

    // Standard getters and setters
}
```


`Author` table:
```sql
+----+------------+
| id | name       |
+----+------------+
|  1 | Mark Twain |
|  2 | Jane Austen|
+----+------------+
```

`Book` table:
```sql
+----+-----------------+
| id | title           |
+----+-----------------+
|  1 | Huckleberry Finn|
|  2 | Pride and Prejudice|
+----+-----------------+
```

`author_book` join table:
```sql
+----------+---------+
| book_id  | author_id |
+----------+---------+
|        1 |       1 |
|        2 |       2 |
+----------+---------+
```



#### JsonManagedReference and backReference
The `@JsonManagedReference` and `@JsonBackReference` annotations are used in the context of JSON serialization/deserialization with the Jackson library in Java, not Java's built-in serialization mechanism. They are used to handle bidirectional relationships and to solve the problem of infinite recursion.

When you have a bidirectional relationship between two entities (e.g., `Parent` <-> `Child`), and you try to serialize them to JSON, you may encounter an infinite loop. This happens because the serializer will keep serializing the parent from the child and the child from the parent endlessly.

Here's an example of what might happen without @JsonManagedReference and @JsonBackReference:
```java
public class Parent {
    private String name;
    private List<Child> children;

    // getters and setters
}

public class Child {
    private String name;
    private Parent parent;

    // getters and setters
}
```

If you attempt to serialize an instance of `Parent` to JSON, the serializer will also try to serialize the `children` list. While serializing a `Child`, it will try to serialize its `parent`, which in turn will serialize its `children`, and so on, leading to infinite recursion.

To prevent this, you can use ==`@JsonManagedReference`== on the forward part of the relationship (==usually the collection side==) and ==`@JsonBackReference`== on the back part of the relationship (==usually the single entity side)==:

In any bidirectional relationship where both sides of the relationship have a direct reference to each other, and you are using Jackson for JSON serialization/deserialization,

# extra info

we handle the exceptions in the service layer


you can use neon tech for trial postgres database






# 7.1 Expense Manager

# 7.2 Spring Data and JPA

ways of accessing data
- pure JDBC (not something taught in the course, but exits)
- using hibernate (using sessions)
- using spring data (using JPAcrudRepo)

![[Pasted image 20240518202539.png]]


In ==Using Spring Data== method, we don't have to do the implementation for the repo layer
Make the interface extend the `CrudRepository<T, ID>` 
	where `T` is the entity and `ID` is the Datatype of the id of the entity

(There is also `JpaRepository<T, ID>` which extends `CrudRepository` but also extends the `ListPagingAndSortingRepository<T, ID>`)

### What changes are required in `application.properties` / yml file, to use spring data?

- Remove hibernate dialect, coz we don't use it now
- Add driver class (mysql/postgres)

#### NOTE:
`findAll()` returns `Iterable<T>` instead of `List/ArrayList<T>`


# 7.3 Spring Data JPA Queries

Now we have APIs given by Spring Data JPA. But what if we require complex queries, 
1. Derived query
2. JPQL
3. Custom Query (Native)

### 1. Derived Query

ex: 

users

| id  | name  | age |
| --- | ----- | --- |
| 1   | John  | 24  |
| 2   | Alice | 25  |
| 3   | Bob   | 25  |

query 1:
```sql
SELECT * FROM users WHERE age = 30 
```
the above sql query if required can be done by 
```java
findByAge(30)
```


query 2:
```sql
SELECT * FROM users WHERE age > 30 
```
the above sql query if required can be done by 
```java
findByAgeGreaterThan(30)
```

Just write the method signatures shown, in the interface
We generally use `JpaRepository` instead of `CrudRepository` because `JpaRepository` is more extensible than `CrudRepository`

### 2. JPQL

JPQL (Java Persistence Query Language)
It is platform independent query language

```java
@Query(JPQL query here)
someListorReturnType<T> someMethodName();
```
you have to write so, in the interface

#### how to write JPQL query?
JPQL query is similar to SQL query. Instead of table name use entity name, making it platform independent

ex:
```java
@Query("SELECT u FROM Users u WHERE u.age = 30 ")
```

but for a generic query
(passing the argument from the method to the query)

Method 1:
```java
@Query("SELECT u FROM User u WHERE u.age = ?1")
List<User> someMethodName(int age);
```

`?1` represents the first arg in the method,
`?2` represents the second arg in the method
and so on...


Method 2:
```java
@Query("SELECT u FROM User u WHERE u.age = :age")
List<User> someMethodName(@Param("age") int age);
```

where use `:id` and then in the arg `@Param("id")` 

### 3. Native query

For complex queries, execution time might be more, hence using native queries can be beneficial to reduce time. 
But using native query, will make the query platform dependent (queries will work for only that language)

![[Pasted image 20240518205548.png]]

```java
@Query(value = "native query here", nativeQuery = true)
someListorReturnType<T> someMethodName();
```


---

### Using `NamedQuery` and `NamedNativeQuery`

These are written on entities, not on methods

```java
@NamedQuery(name="", query="")
```


```java
@NamedNativeQuery(name="", query="", resultClass="")
```
where `resultClass` is the return type of the result of the query 


# 7.4 E-voting application



Solution description
The correct option is (C) Authentication filter -> Authentication manager -> Authentication provider
• Authentication filter: The incoming API request first passes through one or more authentication filters. These filters are responsible
for extracting authentication credentials from the request (e.g., username and password from HTTP headers or tokens) and
creating an authentication object.
• Authentication manager: Once the authentication filter has created the authentication object, it forwards it to the authentication
manager. The authentication manager's role is to verify the authenticity of the credentials provided in the authentication object. It
does this by delegating the authentication to one or more authentication providers.
• Authentication provider: Authentication providers are responsible for performing the actual authentication. They validate the
credentials against the configured user store (e.g., a database, LDAP, or an in-memory store) and return an authenticated user
object if the credentials are valid.



# 8.1 


Authentication vs Authorization


https://docs.spring.io/spring-security/reference/migration-7/configuration.html
```java
 @Bean
  public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(authorize -> authorize
                .requestMatchers("/blog/**").permitAll()
                .anyRequest().authenticated()
            )
            .formLogin(formLogin -> formLogin
                .loginPage("/login")
                .permitAll()
            )
            .rememberMe(Customizer.withDefaults());
    return http.build();
  }

```


#### Form login

This is also the default authentication type when the spring-security dependency is put in `pom.xml`
The password is shown in console and username is `user`


automatically redirected to "http:localhost:8080/login"
- where we need to fill the username and password to enter the site.

to logout: "http:localhost:8080/logout" and click on logout


The code I tried: (to create custom user with username and password)
```java

@Configuration
@EnableWebSecurity
public class UserSecurity {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
        .authorizeRequests(authz -> authz
                .anyRequest().authenticated()
        )
        .formLogin(withDefaults());
        
        return http.build();
    }

    @Bean
    public InMemoryUserDetailsManager userDetailsService() {
        UserDetails user = User.builder()
                .username("user")
                .password(passwordEncoder().encode("password"))
                .build();
            // you can also add roles to user

        return new InMemoryUserDetailsManager(user);
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder(8);
    }
}

```




#### Basic Auth
![[Pasted image 20240501205624.png]]


```java
@Configuration
@EnableWebSecurity
public class UserSecurity {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {


        http
        .authorizeRequests(authz -> authz
                .anyRequest().authenticated()
        )
	        .httpBasic(withDefaults()); /// ==== only change ==
        return http.build();
    }

    @Bean
    public InMemoryUserDetailsManager userDetailsService() {

        UserDetails user = User.builder()
                .username("user")
                .password(passwordEncoder().encode("password"))
                .roles("NORMAL")
                .build();

        return new InMemoryUserDetailsManager(user);
    }


    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder(8);
    }
}

```




# 8.2


loadByUsername -> `DaoAuthenticationProvider` -> `AuthenticationProvider` -> `AuthenticationManager`


https://www.learncodewithdurgesh.com/blogs/jwt-authentication-with-spring-boot-31





# // misc

In Spring Boot with Spring Security, the basic flow for form-based login is as follows:

1. User submits a login form with username and password.
2. The request is intercepted by the `UsernamePasswordAuthenticationFilter`.
3. The filter extracts the username and password from the request and creates an `Authentication` object (usually an instance of `UsernamePasswordAuthenticationToken`).
4. The filter passes the `Authentication` object to the `AuthenticationManager`.
5. The `AuthenticationManager` delegates the authentication process to an `AuthenticationProvider`.
6. The `AuthenticationProvider` validates the credentials, often by querying a userDetailsService to retrieve the user details from a database.
7. If the credentials are valid, the `AuthenticationProvider` creates a fully populated `Authentication` object (including granted authorities/roles).
8. The `Authentication` object is then stored in the `SecurityContext` by the `SecurityContextHolder`.
9. The user is now authenticated, and the request proceeds to the requested resource.
10. For subsequent requests, the `SecurityContextPersistenceFilter` retrieves the `SecurityContext` from the HTTP session, and the user remains authenticated.
11. If authentication fails at any point, an `AuthenticationException` is thrown, and the user is redirected to an error page or a login error page.

Throughout this process, the `SecurityFilterChain` manages the flow of the request through various security filters, each with a specific responsibility (such as CSRF protection, session management, etc.).

It's important to note that Spring Security is highly customizable, and developers can modify this flow to suit their specific needs, such as using custom filters, authentication providers, or even a different authentication mechanism altogether.


To use database-based authentication instead of in-memory authentication, you need to set up a `UserDetailsService` that retrieves user details from a database. You will also need to configure a data source and possibly an `AuthenticationProvider` if you want to customize the authentication process further.

Here's an example of how to configure Spring Security to use a database for authentication:

```java
@Configuration
@EnableWebSecurity
public class UserSecurity {

    @Autowired
    private DataSource dataSource;

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .authorizeRequests(authz -> authz
                .anyRequest().authenticated()
            )
            .formLogin(withDefaults());
        return http.build();
    }

    @Bean
    public UserDetailsService userDetailsService() {
        return new JdbcUserDetailsManager(dataSource);
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder(8);
    }
}
```


In the example above, `JdbcUserDetailsManager` is used as the `UserDetailsService` implementation, which is a JDBC-based user details manager that retrieves user information from a database. You need to provide a `DataSource` bean, which is typically configured in your application properties or a separate configuration class.

The `DataSource` should point to your database, and you should have the necessary tables and data in place for `JdbcUserDetailsManager` to work. By default, `JdbcUserDetailsManager` expects certain table and column names, but you can customize this by setting the appropriate queries on the `JdbcUserDetailsManager` instance.

Here's an example of how you might configure your `DataSource` in `application.properties`:

```
spring.datasource.url=jdbc:mysql://localhost:3306/your_database
spring.datasource.username=db_user
spring.datasource.password=db_password
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
```

Make sure to replace `your_database`, `db_user`, `db_password`, and the driver class name with the appropriate values for your database.

Additionally, if you need to customize the default schema or queries used by `JdbcUserDetailsManager`, you can do so by creating a bean of `JdbcUserDetailsManager` and setting the custom queries:

```java
@Bean
public JdbcUserDetailsManager jdbcUserDetailsManager() {
    JdbcUserDetailsManager jdbcUserDetailsManager = new JdbcUserDetailsManager(dataSource);
    jdbcUserDetailsManager.setUsersByUsernameQuery("SELECT username, password, enabled FROM users WHERE username = ?");
    jdbcUserDetailsManager.setAuthoritiesByUsernameQuery("SELECT username, authority FROM authorities WHERE username = ?");
    return jdbcUserDetailsManager;
}
```

In this example, replace `"SELECT username, password, enabled FROM users WHERE username = ?"` and `"SELECT username, authority FROM authorities WHERE username = ?"` with the actual SQL queries that match your database schema.

You are correct that `JdbcUserDetailsManager` does not have a `setUsersByUsernameQuery()` method. Instead, this method is available in `JdbcDaoImpl`, which is the class that `JdbcUserDetailsManager` extends from.



JWT: https://www.youtube.com/watch?v=KxqlJblhzfI 



Junit 5
https://www.youtube.com/watch?v=6uSnF6IuWIw




Certainly! Understanding `@Mock` and `@InjectMocks` in the context of Mockito can be a bit confusing at first, but once you grasp their roles, it becomes much clearer. Here’s a detailed explanation:

### @Mock
- **Purpose**: `@Mock` is used to create and inject mock instances.
- **Usage**: When you want to mock a dependency or an object in your unit tests.
- **Behavior**: It creates a mock implementation of the class or interface, meaning that calls to the mock instance will be intercepted and can be controlled with Mockito’s stubbing and verification methods.

#### Example
Let's say we have a `UserService` class that depends on a `UserRepository`.

```java
public class UserService {
    private UserRepository userRepository;

    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    public User findUserById(Long id) {
        return userRepository.findById(id);
    }
}
```

In your test class, you can use `@Mock` to mock `UserRepository`.

```java
@RunWith(MockitoJUnitRunner.class)
public class UserServiceTest {

    @Mock
    private UserRepository userRepository;

    @Test
    public void testFindUserById() {
        UserService userService = new UserService(userRepository);
        User mockUser = new User(1L, "John Doe");

        when(userRepository.findById(1L)).thenReturn(mockUser);

        User user = userService.findUserById(1L);

        assertEquals("John Doe", user.getName());
    }
}
```

### @InjectMocks
- **Purpose**: `@InjectMocks` is used to create an instance of the class and inject the mocked dependencies annotated with `@Mock` into it.
- **Usage**: When you want to test a class that has dependencies and you want to inject mocks into the class automatically.
- **Behavior**: It tries to inject mocks either via constructor injection, setter injection, or field injection.

#### Example
Using the same `UserService` and `UserRepository`, you can use `@InjectMocks` to simplify the injection of mocks.

```java
@RunWith(MockitoJUnitRunner.class)
public class UserServiceTest {

    @Mock
    private UserRepository userRepository;

    @InjectMocks
    private UserService userService;

    @Test
    public void testFindUserById() {
        User mockUser = new User(1L, "John Doe");

        when(userRepository.findById(1L)).thenReturn(mockUser);

        User user = userService.findUserById(1L);

        assertEquals("John Doe", user.getName());
    }
}
```

### Key Differences and When to Use Them

- **@Mock**:
  - Use when you need to create mock instances of dependencies.
  - You manually inject these mocks into the class you are testing.
  - More control over how and where the mocks are injected.

- **@InjectMocks**:
  - Use when you want Mockito to handle the injection of mocks automatically.
  - Simplifies test setup by reducing boilerplate code.
  - Ideal for when you have many dependencies to mock and inject.

### Context of Use
- **Unit Testing**: Both `@Mock` and `@InjectMocks` are primarily used in unit tests to isolate the class under test from its dependencies.
- **Spring Boot Applications**: While writing tests for Spring Boot applications, these annotations help in mocking Spring components, repositories, services, etc., making it easier to test the business logic without involving the actual database or external systems.

By understanding and utilizing `@Mock` and `@InjectMocks` appropriately, you can create more maintainable and isolated unit tests for your Spring Boot applications.



Great questions! Let's break this down to clarify what's happening when you call methods on mock objects versus methods on an `@InjectMocks` object.

### Calls on Mock Objects

When you call a method on a mock object created with `@Mock`, **no actual logic from the real object is executed**. Instead, Mockito intercepts the call and you can control its behavior using stubbing (e.g., `when`, `thenReturn`).

#### Example

```java
@Mock
private UserRepository userRepository;

@Test
public void testFindUserById() {
    User mockUser = new User(1L, "John Doe");
    when(userRepository.findById(1L)).thenReturn(mockUser); // Stubbing

    User result = userRepository.findById(1L); // Call on the mock object

    // Verification and assertion
    assertEquals("John Doe", result.getName());
}
```

In this example:
- The call to `userRepository.findById(1L)` doesn't actually query a database.
- The method call returns `mockUser` because we stubbed it with `when(...).thenReturn(...)`.

### Calls on `@InjectMocks` Objects

When you call a method on an object created with `@InjectMocks`, an actual call is made to the real method of that object. However, any dependencies (annotated with `@Mock`) within that object are replaced with mocks.

#### Example

```java
@Mock
private UserRepository userRepository;

@InjectMocks
private UserService userService;

@Test
public void testFindUserById() {
    User mockUser = new User(1L, "John Doe");
    when(userRepository.findById(1L)).thenReturn(mockUser); // Stubbing

    User result = userService.findUserById(1L); // Call on the @InjectMocks object

    // Verification and assertion
    assertEquals("John Doe", result.getName());
}
```

In this example:
- The call to `userService.findUserById(1L)` is an actual call to the `UserService`'s `findUserById` method.
- Inside this method, the call to `userRepository.findById(1L)` is intercepted by the mock, returning `mockUser`.

### Purpose of Unit Tests with Mocks

#### Why Use Mocks?

1. **Isolation**: To test a specific class (`UserService` in this case) without involving its dependencies (`UserRepository`).
2. **Control**: To define how dependencies behave using stubbing (e.g., `when`, `thenReturn`) so you can test different scenarios.
3. **Efficiency**: To avoid the overhead of actual calls to external systems (like databases or web services) which can be slow and difficult to set up.

#### Do They Want an Actual Call or Not?

- **Testing the Class Under Test**: You want to make actual calls to the methods of the class under test (e.g., `UserService`), to verify its behavior.
- **Mocking Dependencies**: You do not want to make actual calls to the dependencies (e.g., `UserRepository`), because:
  - You want to isolate the behavior of the class under test.
  - You want to control the return values and behaviors of these dependencies to create predictable and specific test scenarios.

### Summary

- **Mock Objects (`@Mock`)**: Calls to these objects do not execute actual logic of the class; they return predefined results (stubbing).
- **InjectMocks Objects (`@InjectMocks`)**: Calls to these objects execute the actual logic of the class, but the dependencies inside are mocked to isolate the class's behavior.

In a unit test, the goal is typically to verify the behavior of the class under test by making actual calls to its methods while controlling the behavior of its dependencies using mocks. This ensures you are testing the logic of the class itself, not the interactions with real dependencies.



---
extra

mockito
https://stackoverflow.com/questions/11462697/forming-mockito-grammars

```java
// Setup expectations
when(object.method()).thenReturn(value);
when(object.method()).thenThrow(exception);
doThrow(exception).when(object).voidMethod();


// verify things
verify(object, times(2)).method();
verify(object, times(1)).voidMethod();

```

