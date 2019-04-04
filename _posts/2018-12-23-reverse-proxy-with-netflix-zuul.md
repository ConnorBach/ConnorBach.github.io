---
layout: post
title: "Developing React apps with Spring Boot and Netflix Zuul"
---

## Spring Boot and React

If you use Spring Boot as your standard web serving solution it is fairly easy to build
React applications on top of Spring Boot with a little extra configuration. This post gives an overview of 
the workflow I found to be most optimal for developing React apps on top of Spring Boot.
Additionally, it goes over some of the common pitfalls and provides some solutions for the
most frequent issues. I linked some other helpful posts at the bottom of the page if you require additional details.

## Local development

During local development you will have two different applications. The first is a React application that will run by default on localhost:3000, while
the second is a Spring Boot application that will usually run on localhost:8080.

You will want to take advantage of React's hot reloading feature to get real time changes
on the front-end code. I recommend using Facebook's [create-react-app](https://github.com/facebook/create-react-app) to setup the framework
for your React application.

In order for the two applications to communicate on the same machine you need to add a proxy to the Spring Boot application's
port in the React app package.json.
```
"proxy": "http://localhost:8080"
```

Now you can run each application individually and have the React application make API calls to the Spring Boot app.

## Deploying

First you need to generate the React application output. Run
```
npm run build
```
and copy the output to the /static path in your Spring Boot application. 

Now your Spring Boot server will have the usual Java backend in addition to serving the frontend React app.

You can use the Maven frontend-plugin to automate the npm build and resource copy process.

```
<!-- https://mvnrepository.com/artifact/com.github.eirslett/frontend-maven-plugin -->
<dependency>
    <groupId>com.github.eirslett</groupId>
    <artifactId>frontend-maven-plugin</artifactId>
    <version>1.6</version>
</dependency>
```

## Using Zuul as a Reverse Proxy

To avoid CORS errors now that we have deployed our code we need a tool to proxy our requests. Enter Zuul. 
Netflix has created a tool built around the Spring framework that can act as a reverse
proxy, allowing front-end code to make requests to a backend with a different origin while avoiding
CORS errors.  


To use Zuul in this manner add it to your dependencies.

```
<!-- https://mvnrepository.com/artifact/com.netflix.zuul/zuul-core -->
<dependency>
    <groupId>com.netflix.zuul</groupId>
    <artifactId>zuul-core</artifactId>
    <version>2.1.3</version>
</dependency>
```

Next you need to add the proper Zuul annotation to your main frontend application class.

```
@EnableZuulProxy
@SpringBootApplication
public class FrontEnd {
    public static void main(String[] args) {
        SpringApplication.run(FrontEnd.class, args);
    }
}
```

The final part of the process is to tell Zuul which path it will proxy by editing the application.properties file.

```
zuul.routes.example.path = /example/*
zuul.routes.example.url = http://example-service.com:8080/example
```

This will proxy any requests to the frontend application at the example route to the service running on port 8080 of localhost.
Your React app can now make API calls to relative URLs like "/example" and it will hit the Spring Boot backend.

## Additional Resources
[Simple CRUD App with React and Spring Boot](https://developer.okta.com/blog/2018/07/19/simple-crud-react-and-spring-boot)

[Microservices Routing with Zuul Proxy](https://springbootdev.com/2018/02/01/microservices-request-routing-with-zuul-proxy-spring-boot-spring-cloud-netflix-zuul/)

[Using Create-React-App with a Server](https://www.fullstackreact.com/articles/using-create-react-app-with-a-server/)

[Spring Rest with Zuul Proxy](https://www.baeldung.com/spring-rest-with-zuul-proxy)

I hope this post gives a decent overview for an example workflow for developing React apps on Spring Boot.

Feel free to reach out if you have any questions!
