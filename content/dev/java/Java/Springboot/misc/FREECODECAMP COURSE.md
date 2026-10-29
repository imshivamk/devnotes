1. install intellijidea and java
2. mongodb atlas, download compass, connection
3. create database - movies-api-db, collection - movies
4. import data into db from movies.json

## 01 spring initialization

1. go to spring initializer
	1. maven - build tool
	2. lang - java
	3. springboot - 3.0.1
	4. Group (in reverse domain format) : com.shivam / dev.shivam
	5. artifact/name - movies
	6. Packaging - `.jar`
2. packages : 
	1. Lombok
	2. spring web
	3. spring data mongodb
	4. devtools
3. GENERATE - it will download the .zip file of project. extract and open in intellij

## 02 basic spring rest controller/ endpoint

```java
package com.shivam.movies
import ...

@SpringBootApplication
@RestController

public class MovieApp{

	public static void main(String[] args){
		SpringApplication.run(MovieApp.class, args);
	}
	
	@GetMapping("/")
	public String apiRoot(){
		return "Hello world";
	}
}
```

## env / config /  properties

- **Use environment variables for dev/test** and secret managers for prod.
- AWS secrets manager for cloud
- add .env dependency from maven

`pom.xml`
```xml
<!-- https://mvnrepository.com/artifact/me.paulschwarz/spring-dotenv -->  
<dependency>  
    <groupId>me.paulschwarz</groupId>  
    <artifactId>spring-dotenv</artifactId>  
    <version>4.0.0</version>  
</dependency>
```


`/.env`
```
MONGO_DATABASE="movies-api-db"  
MONGO_USER="imshivamk"  
MONGO_PASSWORD="r3TCu3p6FwUgNpwu"  
MONGO_CLUSTER="cluster0.qi68c.mongodb.net"
```

*MAKE SURE TO Add to .gitignore*

`resources/application.properties`
```
spring.application.name=movies  
spring.data.mongodb.database=${MONGO_DATABASE}  
spring.data.mongodb.uri=mongodb+srv://${MONGO_USERNAME}:${MONGO_PASSWORD}@${MONGO_CLUSTER}/${MONGO_DATABASE}
```



## directory structure

```
src/main/java/
	com.shivam.movies/
		Movie.java
		MovieController.java
		Review.java
		MoviesApplication.java
	resources/
		application.properties
.env
.gitignore
pom.xml
```

## Models
`Movie.java`
```java

@Document(collection = "movies")  
@Data  
@AllArgsConstructor  
@NoArgsConstructor  
public class Movie {  
  
    @Id  
    private ObjectId id;  
  
    private String imdbId;  
    private String releaseDate;  
    private String trailerLink;  
    private String poster;  
  
    private List<String> genres;  
    private List<String> backdrops;  
  
    // manual reference relationship  
    // only id is returned    @DocumentReference  
    private List<Review> reviewIds;  
  
}
```

`Review.java`
```java
@Document(collection="reviews")
@Data
@AllArgsConstructor
@NoArgsConstructor

private class Review{
	@Id
	private ObjectId id;
	private String body;
}
```

## Controllers

`movieController.java`
```java
@RestController  
@RequestMapping("/api/v1/movies")  
public class MovieController {  
  
    @GetMapping  
    public String allMovies(){  
        //return "All movies : ";  
    }  
    
    @GetMapping  
	public ResponseEntity<String> allMovies(){  
	    return new ResponseEntity<>("All movies", HttpStatus.OK);  
	}
}
```
