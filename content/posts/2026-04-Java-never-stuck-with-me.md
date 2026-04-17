+++
title = 'Java Never Stuck with Me'
date = 2026-04-17T17:07:58+09:00
draft = true
+++

It might come as a surprise for anyone who knew me personally, but I used to despise programming. I am what can be considered a late bloomer. My first introduction to a proper programming was in first year of university: Algorithm and Data Structure class. We used C back then and I can't tell you how much I hated it. Not the class itself, but making the C language do what I want it to do. Google was not as helpful back then, I did not even remember if stack overflow was around, and no LLM for obvious reason. I fight this battle only with the good old "Introduction to Algorithms" book.

Then come the OOP class. Despite my struggle with C, I like the Algorithm class so much. It felt just like playing puzzle. And the satisfaction I got from getting the C language to finally work is immense. But boy the OOP class crank that up.

The OOP class was taught in Java. If I hated C before, I hated Java even more. Like, everywhere I look, doing a simple thing in Java requires so much ceremony.

```java
// Filter a list of numbers, double them, print the result
import java.util.ArrayList;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<Integer> numbers = new ArrayList<>();
        numbers.add(1); numbers.add(2); numbers.add(3); numbers.add(4); numbers.add(5);

        List<Integer> result = new ArrayList<>();
        for (int n : numbers) {
            if (n % 2 == 0) {
                result.add(n * 2);
            }
        }
        System.out.println(result);
    }
}
```

I haven't even started on the logic, but I already wrote 10 lines of scaffolding.

The other classes was similar.

Web Programming? PHP

Image Processing? Matlab

Neural Network? again, Matlab

All these classes are interesting, I loved learning those. But the toolings are from 1950s and it shows. At lowest point of my study, I dreaded having to work in the field I like, then have to use these ancient tools everyday till the day I die.

Until I get to my first job.

With Huawei, I got reunited with Java again, and it was nothing like my class. This Java I was dealing with was used to power charging systems of a telco operators, the ones that dictates how long we can make a call, how much SMS we are allowed to send, and how much data we are allowed to send/receive. It lives in 16 clusters of gateway, 52 clusters of core systems, and 23 clusters of DB. I finally get why Java is important. It is still ugly MF, don't get me wrong. But I get why we use this when reliability is what we are looking for.

One, because it has been around since 1950, it evolved together with a lot of standard, and it has backward compatibility with the most obscure of standard that no one aside from one ATM in Bandung still uses. Two, Java has everything that telco industry needed. They need a hot deployment (swap a business rule without a call dropping), long-running process stability, and mature tools for heap or garbage collection handling. A JavaOutOfMemory error is easy to debug with a full stack trace.

Then I started my masters. Remember when I said I loved the classes but hated the tools? Turns out that doesn't have to be the case at all. My first class was Big Data Processing, and I got introduced to Scala. A language that runs on JVM, but the syntax is beautiful.

What do you mean I can write this and it will work?

```scala
// Scala
// Same thing: Filter a list of numbers, double them, print the result
val numbers = List(1, 2, 3, 4, 5)
val result = numbers.filter(_ % 2 == 0).map(_ * 2)
println(result)
```

Then I also got introduced to python in the same class.

```python
# Python
# Very like Scala: Filter a list of numbers, double them, print the result
numbers = [1, 2, 3, 4, 5]
result = [n * 2 for n in numbers if n % 2 == 0]
print(result)
```

Then my classmate introduced me to node.js for web development.

Then ofcourse I bought Udemy courses to learn about these tools more. I can work on the problem I like and the tool wont fight me anymore? Sign me up, bro!

Then a few years later this is what I actually ended up doing. I used python daily for statistical analysis. I used Scala for the production grade data analytics pipeline. When we need to gave API to a downstream system, I used flask, then fastapi. Turns out, when I don't have to fight the tools, I'm actually happy doing this everyday till the day I die.
