---
layout: post
title:  "Micronaut OpenAPI"
date:   2021-03-01 00:00:00 +0800
categories: framework java maven openapi micronaut 
---
As we all know [Spring][spring] there is now a lightweigth and claimed more efficient framework called [Micronaut][micronaut]
The key difference is it instruments the code AOT (ahead of time) and so ensure less memory footprint (less caching, less dymanic proxy to be created) and so a quicker start time.

<div class="alert alert-success d-flex align-items-center" role="alert">
  <svg class="bi flex-shrink-0 me-2" width="24" height="24" role="img" aria-label="Success:"><use xlink:href="#check-circle-fill"/></svg>
  <div class="blogalert">
    Note this sample application can be found on my git repository, check it out <a href="https://github.com/kdefombelle/micronaut-sample">here</a>
  </div>
</div>


## Cli Installation
Micronaut has a CLI to scaffold your application

<div class="howto">
<ol>
<li>install micronaut-cli using a package manager as per <a href="https://micronaut-projects.github.io/micronaut-starter/latest/guide/index.html#installChocolatey">Micronaut documentation</a></li><br/>

<div class="row">
  <div class="col-sm-6">
    <div class="card">
      <div class="card-body">
        <h5 class="card-title">Windows</h5>
        <p class="card-text">Using <a href="https://chocolatey.org/">Chocolatey</a><br/>
            <br/><code>choco install micronaut</code>
         </p>
      </div>
    </div>
  </div>
  <div class="col-sm-6">
    <div class="card">
      <div class="card-body">
        <h5 class="card-title">Ubuntu</h5>
        <p class="card-text">
          With <a href="https://sdkman.io/">SDKMAN</a><br/><br/>
          <code>sdk install micronaut</code>
        </p>
      </div>
    </div>
  </div>
</div>
<br/>
<li>ensure <code>$MICRONAUT_CLI_HOME/bin</code> to <code>PATH</code>; it will be referred as <code>mn</code></li>
<li>check installation <code>mn --version</code></li>
</ol>
</div>
<br/>

## Sample Application

You can use Mirconaut CLI to initiate your application
```sh
> mn create-app hello-world --build maven --features openapi
```

The project is scaffolded and the main class is generated
```java
public class Application {

  public static void main(String[] args) {
    Micronaut.run(Application.class, args);
  }
}
```

You can start adding a Controller to spawn a server side application
```java
@Controller("/service")
public class ServiceController {

  @Post("execute")
  @Status(HttpStatus.CREATED)
  public Single<Result> execute(@Body Request request) {
    Result result = new Result();
    result.setValue(request.getValue() * 2);
    return Single.just(result);
  }

}
```

To compile or build simply run `mvn compile` or `mvn install`, or use `mn`
```sh
mvnw mn:run
```

### Maven Dependencies
<<<<<<< HEAD
<<<<<<< HEAD
>>>>>>> image optimisation
=======
>>>>>>> merge image optim and minor changes
=======
>>>>>>> ff22a9bd014e6f9bdd540cee88f173c63a09f8dd

In in your maven pom.xml
- add compile dependencies

```xml  
<dependency>
	<groupId>javax.annotation</groupId>
	<artifactId>javax.annotation-api</artifactId>
	<scope>compile</scope>
</dependency>
<dependency>
	<groupId>io.swagger.core.v3</groupId>
	<artifactId>swagger-annotations</artifactId>
	<scope>compile</scope>
</dependency>
```

- configure annotation processor

```xml
<plugin>
	<groupId>org.apache.maven.plugins</groupId>
	<artifactId>maven-compiler-plugin</artifactId>
	<configuration>
		<annotationProcessorPaths combine.children="append">
			<path>
				<groupId>io.micronaut.openapi</groupId>
				<artifactId>micronaut-openapi</artifactId>
				<version>${micronaut.openapi.version}</version>
			</path>
		</annotationProcessorPaths>
		<compilerArgs>
			<arg>-Amicronaut.processing.group=fr.kdefombelle.app</arg>
			<arg>-Amicronaut.processing.module=app-service</arg>
		</compilerArgs>
	</configuration>
</plugin>
```

## Swagger
Reference documentation is [Micronaut Open API][micronaut-openapi]

In your application properties point swagger static resources
```yaml
micronaut:
  application:
    name: sample
  router:
    static-resources:
      swagger:
        paths: classpath:META-INF/swagger
        mapping: /swagger/**
      swagger-ui:
        paths: classpath:META-INF/swagger/views/swagger-ui
        mapping: /swagger-ui/**
```

## Document the API
<<<<<<< HEAD
<<<<<<< HEAD
>>>>>>> image optimisation
=======
>>>>>>> merge image optim and minor changes
=======
>>>>>>> ff22a9bd014e6f9bdd540cee88f173c63a09f8dd
```java
@Operation(
    summary = "Double value",
    description = "Simple webservice which doubles a value provided by the requester")
  @ApiResponse(
    responseCode = "201",
    description = "Response object",
    content = @Content(mediaType = "application/json",
      schema = @Schema(type = "Response")
    )
  )
  @ApiResponse(
    responseCode = "400",
    description = "Response object",
    content = @Content(
      mediaType = "application/json",
      schema = @Schema(type = "Response")
    )
  )
  @Post("execute")
  @Status(HttpStatus.CREATED)
  public Single<Result> execute(@Body Request request) {
    Result result = new Result();
    result.setValue(request.getValue() * 2);
    return Single.just(result);
  }
```

## Enable Swagger
Reference documentation is [Micronaut Open API][micronaut-openapi]

Create an `openapi.properties` file at the root of your project containing
```sh
swagger-ui.enabled=true
```

In your application properties point swagger static resources, e.g.: in application.yml
```yaml
micronaut:
  application:
    name: sample
  router:
    static-resources:
      swagger:
        paths: classpath:META-INF/swagger
        mapping: /swagger/**
      swagger-ui:
        paths: classpath:META-INF/swagger/views/swagger-ui
        mapping: /swagger-ui/**
```

## Visualise
To access swagger you can run your app and go to URL http://localhost:8080/swagger-ui
<<<<<<< HEAD
<<<<<<< HEAD
>>>>>>> image optimisation
=======
>>>>>>> merge image optim and minor changes
=======
>>>>>>> ff22a9bd014e6f9bdd540cee88f173c63a09f8dd

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/assets/img/2021-03-01-micronaut-swagger.png">
    </div>
</div>



[spring]: <https://spring.io/>
[chocolatey]: <https://chocolatey.org/>
[micronaut-openapi]: <https://micronaut-projects.github.io/micronaut-openapi/latest/guide/index.html/>
[micronaut]: <https://micronaut.io/>
