+++
title = 'Kotlin, the JVM I like'
date = 2026-04-17T19:49:44+09:00
+++

People a lot smarter than me will have no problem building complex applications in Java. If you can tame all the ceremonies, Java can be used to create great things. I saw that first-hand. Huawei OCS was built in Java. A fork of Spring Boot was what we used in DANA as our core payment engine. It is considered the King of financial services everywhere in the world.

Now, while I understood some Java, I never really managed to build any serious work with it. I mentioned how ugly Java looked to me. That was mainly because I learned by copying. In order to understand a programming concept, the most optimal way for me to learn is to find an example, then rewrite it down by hand. I will copy one line, run it, see how far that runs or hit a wall, try to understand why that happens, then add the next line by hand again. I can learn very fast doing this.

Now the problem with doing this with Java is that the language requires you to do an exact dance before letting me do anything meaningful at all. The first thing I need to write in Java, the hello world, looked like this.

```java
public class Scratch {
    public static void main(String[] args) {
        System.out.println("hello world");
    }
}
```

Just printing things, I have to already absorb `public`, `class`, `static`, `void`.

Contrast with Python.

```python
print("hello world")
```

Of course I'm not saying that Python is inherently a better language. But for my style of learning, Python syntax made it easier. I can just google, "correlation analysis between two variables".

```python
import numpy as np

sales = [100, 120, 135, 160, 180, 210, 240]
ads =   [10,  12,  15,  20,  22,  28,  35]

print(np.corrcoef(sales, ads)[0, 1])
```

And with Java, just adding the numpy equivalent requires the extra barrier of setting up Maven.

```xml
<!-- pom.xml -->
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-math3</artifactId>
    <version>3.6.1</version>
</dependency>
```

```java
// src/main/java/Correlation.java
import org.apache.commons.math3.stat.correlation.PearsonsCorrelation;

public class Correlation {
    public static void main(String[] args) {
        double[] sales = {100, 120, 135, 160, 180, 210, 240};
        double[] ads   = {10,  12,  15,  20,  22,  28,  35};

        PearsonsCorrelation correlation = new PearsonsCorrelation();
        double r = correlation.correlation(sales, ads);
        System.out.printf("r=%.3f%n", r);
    }
}
```

Then `mvn compile exec:java -Dexec.mainClass=Correlation` assuming I set up Maven correctly the first time, which it rarely is, and that means an extra thing I need to fix.

I can provide tons more examples where Python is more at home for me. But this write-up is not about Python. It's about the embodiment of Java that __I__ like, Kotlin.

I found Kotlin just a few months ago, when I realized a project I worked on in Python started hitting problems considered solved by Spring Boot. I'm talking about @Transactional, garbage collection, and strict typing which in Python requires Pydantic. During initial research, I found out that Spring Boot has started supporting Kotlin extensively. It is considered a first-class citizen now. And what's even better, all snippets look right up my alley.

Correlation function?

```kotlin
@file:DependsOn("org.apache.commons:commons-math3:3.6.1")

import org.apache.commons.math3.stat.correlation.PearsonsCorrelation

val sales = doubleArrayOf(100.0, 120.0, 135.0, 160.0, 180.0, 210.0, 240.0)
val ads   = doubleArrayOf(10.0,  12.0,  15.0,  20.0,  22.0,  28.0,  35.0)

println(PearsonsCorrelation().correlation(sales, ads))
```

A class that holds some data, which in Java requires getters/setters and in Python requires Pydantic?

In Kotlin:

```kotlin
data class Person(val name: String, val age: Int)
```

And what about the thing I need most, Spring Boot?

In Java:

```java
@RestController
@RequestMapping("/users")
public class UserController {

    @GetMapping("/{userId}")
    public ResponseEntity<Map<String, Object>> getUser(@PathVariable Long userId) {
        Map<String, Object> response = new HashMap<>();
        response.put("id", userId);
        response.put("name", "Andy");
        return ResponseEntity.ok(response);
    }

    @PostMapping
    public ResponseEntity<Map<String, Object>> createUser(@RequestBody UserRequest request) {
        Map<String, Object> response = new HashMap<>();
        response.put("message", "Created");
        response.put("user", request);
        return ResponseEntity.ok(response);
    }
}

// Separate file
public class UserRequest {
    private String name;
    private String email;
    // getters, setters, constructors...
}
```

While in Kotlin:

```kotlin
@RestController
@RequestMapping("/users")
class UserController {

    data class User(val name: String, val email: String)

    @GetMapping("/{userId}")
    fun getUser(@PathVariable userId: Long) =
        mapOf("id" to userId, "name" to "Andy")

    @PostMapping
    fun createUser(@RequestBody user: User) =
        mapOf("message" to "Created", "user" to user)
}
```

Now I start going at the fun parts of JVM instead of repeatedly asking about fundamental things, which I did a lot when dealing with Java. I now research about L1+L2 caching. Event-driven design, which is a ton of pain in Python, has first-class support in Spring Boot. Getting ready for microservices? In Spring Boot, the ecosystem is absurdly comprehensive. From service discovery, config server, and circuit breakers to much more that I never realized. In Python and FastAPI, I have to glue in various third-party libraries. In Spring Boot, I just need to use `@EnableServiceDiscovery`, `@CircuitBreaker`, etc.

I started designing the system to be agnostic. If I need to run heavy-duty machine learning, which of course I do, then I put in a Python sidecar with async execution.

If I dont need all the extra buff provided by Spring Boot? Ktor.

Spring Boot backend too heavy? a dedicated backend-for-frotend in golang if I want.

Its been a very long while since I last sat down and spent hours studying. The past few month has re-ignited this part. The language is fun, the ecosystem is fun. Not to mention nowadays I dont have to hunt for Udemy courses that, while useful, usually cover a lot more or a bit less than what I need. Learning with LLMs helped me learn A LOT faster now. I can just ask it to do one specific things, and it will give me the exact example that I need. In the order I expect to encounter them in real life. The imperfection that many people contribute to sloppiness, is actually helpful for learning. I expect something to not work, so the cycle of reading to understand each line, copying one line by hand, running into an error, solving it and get it work is my teacher.