---
title: "Choosing the Optimal Approach for API Automation"
datePublished: Sat Apr 20 2024 10:30:09 GMT+0000 (Coordinated Universal Time)
cuid: clv7yk66n000b0ajpemm077sk
slug: choosing-the-optimal-approach-for-api-automation
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1713606126006/37d9f80e-1ffd-4fdf-bb56-8ceb12a12388.png
tags: spring, testautomation, rest-assured, apitestautomation

---

### Introduction

As an automation engineer, testing APIs is a significant part of our role. While there are numerous tools available for this purpose, choosing the right one can significantly impact our testing efficiency. In this blog post, we'll explore several options, including native clients like `HttpClient` and `HttpURLConnection`, as well as `REST Assured`, `Spring RestTemplate`, `Spring WebClient`, and `Apache HttpClient`.

Let's explore a simple use case and add tests using all the options we have.

Consider our API URL [`https://jsonplaceholder.typicode.com/users/1`](https://jsonplaceholder.typicode.com/users/1) which returns the below JSON response.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1713589659898/acbd285c-9ab2-40e1-b221-7df17560fbc9.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1713589933136/37acce18-8a51-4916-8848-3bbe9464cdac.png align="center")

So the test is to:

* Invoke the GET API using any client
    
* Get the response & validate status code is `200`
    
* Validate some parts of the response, maybe `name`
    

Let's see how we can accomplish this with different approaches.

`The complete code for this tutorial with all examples discussed can be found on` [`GitHub`](https://github.com/rakesh-vardan/restassured-vs-native-clients)`.`

### **1\. HttpURLConnection**

[`HttpURLConnection`](https://docs.oracle.com/javase/8/docs/api/java/net/HttpURLConnection.html) has been a part of the Java standard library since Java 1.1. It's a blocking API, meaning it will hold the thread until it gets the response. It only supports HTTP/1.1. Its API is also low-level, which means we would need to write more code to do the same tasks as with other options.

```java
    @Test
    void testWithHttpURLConnection() throws IOException {
        // prepare request
        URL url = new URL(this.URL);
        HttpURLConnection connection = (HttpURLConnection) url.openConnection();
        connection.setRequestMethod("GET");

        // send request
        connection.connect();

        // validate response
        assertEquals(200, connection.getResponseCode());
        assertEquals("application/json; charset=utf-8", connection
                .getHeaderField("Content-Type"));

        BufferedReader reader = new BufferedReader(new InputStreamReader(connection.getInputStream()));
        String line;
        StringBuilder response = new StringBuilder();
        while ((line = reader.readLine()) != null) {
            response.append(line);
        }
        reader.close();

        assertTrue(response.toString().contains("Leanne Graham"));
        connection.disconnect();
    }
```

This JUnit test uses Java's native `HttpURLConnection` to send a GET request to the given URL and validates the response. It first creates a new `URL` object and opens a connection to it using `HttpURLConnection`. The request method is set to `GET`. Then, it sends the request by calling `connect()`. It validates the response by checking that the response code is `200`, indicating success and that the `Content-Type` header is `application/json; charset=utf-8`. It then reads the response body using a `BufferedReader` and checks that the response body contains the string "Leanne Graham". If any of these checks fail, the test will fail. If all checks pass, the test will pass. The test may throw an IOException if there's a problem sending the request or receiving the response. After all operations, it disconnects the connection by calling `disconnect()`.

### **2\. HttpClient**

Introduced in Java 11, [`HttpClient`](https://docs.oracle.com/en%2Fjava%2Fjavase%2F11%2Fdocs%2Fapi%2F%2F/java.net.http/java/net/http/HttpClient.html) is a modern and flexible API that supports both HTTP/1.1 and HTTP/2. It provides both synchronous (blocking) and asynchronous (non-blocking) programming models. However, it's more of a low level and doesn't provide the same level of functionality for testing as some other options.

```java
    @Test
    void testWithHttpClient() throws IOException, InterruptedException {
        // prepare request
        HttpClient client = HttpClient.newHttpClient();
        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create(this.URL))
                .build();

        // send request
        HttpResponse<String> response = client.send(request,
                HttpResponse.BodyHandlers.ofString());

        // validate response
        assertEquals(200, response.statusCode());
        assertEquals("application/json; charset=utf-8", response.headers()
                .firstValue("Content-Type").get());
        assertTrue(response.body().contains("Leanne Graham"));
    }
```

This test uses Java's native `HttpClient` to send a GET request to a specified URL and validates the response. It first creates a new `HttpClient` instance and builds an `HttpRequest` for the specified URL. Then, it sends the `HttpRequest` using the `HttpClient` and receives the `HttpResponse`, specifying that the response body should be treated as a String. Then it validates the response as per our test scenario.

### **3\. Apache HttpClient**

[`Apache HttpClient`](https://hc.apache.org/httpcomponents-client-4.5.x/index.html) is a robust, feature-rich, and flexible library that provides almost all the functionality needed to send HTTP requests and handle HTTP responses. It supports both blocking and non-blocking I/O models and provides full control over the HTTP protocol's details.

*Make sure you're adding the appropriate Maven/Gradle dependency for Apache HttpClient in your project. For Maven, you can use:*

```xml
    <dependency>
      <groupId>org.apache.httpcomponents.client5</groupId>
      <artifactId>httpclient5</artifactId>
      <version>5.3.1</version>
    </dependency>
```

```java
    @Test
    void testWithApacheHttpClient() throws ProtocolException, IOException {
        // prepare request
        CloseableHttpClient httpClient = HttpClients.createDefault();
        HttpGet request = new HttpGet(this.URL);

        // send request
        CloseableHttpResponse response = httpClient.execute(request);

        // validate response
        assertEquals(200, response.getCode());
        assertEquals("application/json; charset=utf-8",
                response.getHeader("Content-Type").getValue());
        assertTrue(EntityUtils.toString(response.getEntity())
                .contains("Leanne Graham"));
        httpClient.close();
    }
```

This test uses Apache's `CloseableHttpClient` to send a GET request to a specified URL and validates the response. It first creates a new `CloseableHttpClient` and builds a `HttpGet` request for the specified URL. Then, it sends the `HttpGet` request using the `CloseableHttpClient` and receives the `CloseableHttpResponse`. It validates the response similar to the previous examples. After all operations, it closes `httpClient` to free up system resources.

### **4\. Spring RestTemplate**

[`RestTemplate`](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/client/RestTemplate.html) is a synchronous HTTP client that was part of the Spring Framework. It provides a higher-level, more user-friendly API compared to `HttpClient` and `HttpURLConnection`. However, as of Spring 5, RestTemplate is in maintenance mode, and it's suggested to use the non-blocking `WebClient` instead.

*Make sure you're adding the appropriate Maven/Gradle dependency for Spring Web in your project. For Maven, you can use:*

```xml
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-web</artifactId>
      <version>6.1.6</version>
    </dependency>
```

```java
    @Test
    void testWithSpringRestTemplate() {
        // prepare and send request
        RestTemplate restTemplate = new RestTemplate();
        ResponseEntity<String> response = restTemplate.getForEntity(this.URL, String.class);

        // validate response
        assertEquals(200, response.getStatusCodeValue());
        assertEquals("application/json; charset=utf-8", response.getHeaders()
                .getFirst("Content-Type"));
        assertTrue(response.getBody().contains("Leanne Graham"));
    }
```

This test uses Spring's `RestTemplate` to send a GET request to a specified URL and validates the response. It first creates a new `RestTemplate` and sends a GET request to the specified URL, receiving the response as a `ResponseEntity<String>`. Then it validates the response as per our test scenario.

### **5\. Spring WebClient**

[`WebClient`](https://docs.spring.io/spring-framework/reference/web/webflux-webclient.html) is a non-blocking, reactive web client introduced in Spring 5 as part of the WebFlux module. It's designed to work in a non-blocking way and is suitable for use in reactive applications where traditional blocking I/O operations are inefficient.

*Make sure you're adding the appropriate Maven/Gradle dependency for Spring Webflux in your project. For Maven, you can use:*

```xml
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webflux</artifactId>
      <version>6.1.6</version>
    </dependency>
```

```java
    @Test
    void testWithSpringWebClient() {
        // prepare and send request
        WebClient webClient = WebClient.create();
        ClientResponse response = webClient.get()
                .uri(this.URL)
                .exchange()
                .block();

        // validate response
        assert response != null;
        assertEquals(HttpStatus.OK, response.statusCode());
        assertEquals("application/json;charset=utf-8",
                Objects.requireNonNull(response.headers().asHttpHeaders().getContentType())
                        .toString());
        assertTrue(Objects.requireNonNull(response.bodyToMono(String.class).block())
                .contains("Leanne Graham"));
    }
```

This test uses Spring's `WebClient` to send a GET request to a specified URL and retrieve the response. It creates an instance of `WebClient`, sends the request, and blocks until the response is received. It validates the response similar to the previous examples. The `Objects.requireNonNull()` calls are used to ensure that the `getContentType()` and `bodyToMono(String.class).block()` methods do not return `null`. If they do, a `NullPointerException` will be thrown.

### **6\. REST Assured**

[`REST Assured`](https://rest-assured.io/) is an open-source Java library that simplifies the testing and validation of REST APIs. It provides a high-level, fluent API for sending HTTP requests and validating responses, making it an ideal tool for testing in a behavior-driven development (BDD) style. It is built on top of `Apache HTTP Client` for handling HTTP requests and responses and `Groovy` for its syntax and language features. It also uses other libraries such as `GSON` and `Jackson` for JSON, and `JAXB` for XML.

*Make sure you're adding the appropriate Maven/Gradle dependency for REST Assured in your project. For Maven, you can use:*

```xml
    <dependency>
      <groupId>io.rest-assured</groupId>
      <artifactId>rest-assured</artifactId>
      <version>5.4.0</version>
      <scope>test</scope>
    </dependency>
```

```java
    @Test
    void testWithRESTAssured() {
        given().
                baseUri(this.URL).       // prepare request
        when()
                .get().                  // send request
        then()
                .statusCode(200).and()   // validate response
                .body(containsString("Leanne Graham")).and()
                .header("Content-Type", "application/json; charset=utf-8");
    }
```

This test uses REST Assured to send a GET request to a specified URL and validate the response. The `baseUri(this.URL)` method sets the base URL for the request. The `get()` method sends the GET request. The `statusCode(200)` assertion checks that the status code of the response is 200, indicating success. The `and()` methods are used for readability and do not affect the execution of the test.

As we see with our examples, writing API tests & adding assertions with REST Assured is very easy. It stands out for its ease of use, powerful validation features, support for various types of authentication, and flexibility. It's built on top of the `Apache HttpClient`, which means we can customize it to suit our needs. We can add filters, define the request and response specifications, and more. Using REST Assured, we can write efficient tests with very few lines of readable code.

REST Assured offers several benefits for testing RESTful APIs:

1. **BDD Format:** REST Assured supports Behavior Driven Development (BDD) format, making it easier to write, read, and understand tests. This also facilitates communication between developers, testers, and non-technical stakeholders.
    
2. **DSL:** REST Assured provides a Domain Specific Language (DSL) for writing tests, which simplifies the process of writing complex HTTP requests.
    
3. **Specification:** It allows you to define detailed specifications for the API, which can be used to generate detailed reports and documentation.
    
4. **Schema Validation:** REST Assured supports schema validation for both JSON and XML, ensuring that the API responses match the expected structure.
    
5. **Ease of Use:** REST Assured is designed to be simple and intuitive, making it easy for beginners to get started with API testing.
    
6. **Integration:** It integrates seamlessly with existing Java-based testing ecosystems, including JUnit and TestNG.
    
7. **Flexibility:** REST Assured supports a variety of HTTP methods, including GET, POST, PUT, DELETE, OPTIONS, PATCH, and HEAD, and can handle any type of MIME type, providing flexibility in testing different types of APIs.
    
8. **Authentication Support:** REST Assured supports various types of authentication, such as Basic, Digest, Form, and OAuth, making it easier to test APIs that require user authentication.
    
9. **Detailed Logging:** REST Assured provides detailed logging capabilities, which can be very helpful for debugging and understanding the flow of requests and responses.
    
10. **Built-in Support for Hamcrest Matchers:** REST Assured includes built-in support for Hamcrest matchers, which allows for more readable and flexible assertions. This enhances the descriptiveness of our tests, making them easier to read and maintain.
    

Overall, REST Assured provides a comprehensive and user-friendly framework for testing RESTful APIs, making it a popular choice among developers and testers.

Here's a comparison table for all the mentioned solutions:

| Feature/Tool | HttpURLConnection | HttpClient | Apache HttpClient | Spring RestTemplate | Spring WebClient | REST Assured |
| --- | --- | --- | --- | --- | --- | --- |
| HTTP/2 Support | No | Yes | Yes | No | Yes | Yes |
| Non-Blocking I/O | No | Yes | Yes | No | Yes | No |
| WebSocket Support | No | Yes | No | No | Yes | No |
| OAuth2 Support | No | No | Yes | Yes | Yes | Yes |
| JSON Support | No | No | No | Yes | Yes | Yes |
| XML Support | No | No | No | Yes | Yes | Yes |
| Fluent API | No | Yes | Yes | Yes | Yes | Yes |
| Built-in Response Validation | No | No | No | No | No | Yes |
| BDD Style Support | No | No | No | No | No | Yes |
| Part of Java Standard Library | Yes | Yes | No | No | No | No |

### Conclusion

While native clients like `HttpClient` and `HttpURLConnection`, as well as libraries like `Spring RestTemplate`, `Spring WebClient`, and `Apache HttpClient` have their specific uses, `REST Assured` is a superior choice for test automation engineers looking to efficiently test REST APIs. Its high-level, fluent API, powerful validation features, and support for various types of authentication make it a versatile and efficient tool for API test automation. So, if you're an automation engineer looking to streamline your API testing, give REST Assured a try!