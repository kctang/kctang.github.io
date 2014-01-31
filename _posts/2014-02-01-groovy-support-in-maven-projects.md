---
layout: post
title: Groovy Support in Maven Projects
date: 2014-02-01
categories: maven groovy java
---

Groovy has a unique quality that other dynamic/scripting languages does not - you can write plain Java code in a Groovy class.

This attribute makes it an excellent choice for Java developers trying to pick up this dynamic language - start by writing Java and ease into Groovy's style as you practice.

To make the transition even smoother, you can mix Java classes with Groovy classes in Maven projects. All you need is to configure your project's `pom.xml` to "recognize" Groovy classes.

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <!-- reference - http://groovy.codehaus.org/Groovy-Eclipse+compiler+plugin+for+Maven -->

    <dependencies>
        <dependency>
            <groupId>org.codehaus.groovy</groupId>
            <artifactId>groovy-all</artifactId>
            <version>2.1.7</version>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.1</version>
                <configuration>
                    <source>1.6</source>
                    <target>1.6</target>
                    <compilerId>groovy-eclipse-compiler</compilerId>
                </configuration>
                <dependencies>
                    <dependency>
                        <groupId>org.codehaus.groovy</groupId>
                        <artifactId>groovy-eclipse-compiler</artifactId>
                        <version>2.8.0-01</version>
                    </dependency>
                    <dependency>
                        <groupId>org.codehaus.groovy</groupId>
                        <artifactId>groovy-eclipse-batch</artifactId>
                        <version>2.1.5-03</version>
                    </dependency>
                </dependencies>
            </plugin>
        </plugins>
    </build>
</project>
```

That's it.

Some tips when doing this:

* If you are new to Groovy, it is good to know the [differences from Java](http://groovy.codehaus.org/Differences+from+Java).

* Start with writing unit tests in Groovy where its syntactic sugar will make writing unit tests much more concise and enjoyable. This feature alone can justify enabling Groovy support in a Maven project.

* Turn any existing Java class into a Groovy class by just renaming the file to .groovy. Note: This can be done via IntelliJ's refactor "Rename File..." action.

* If you are not familiar with the dynamic nature of Groovy, just code in plain Java. No need to stress yourself out trying to be a pro-Groovy coder quickly.

* IntelliJ's refactoring feature "Convert to Java" can be useful to convert Groovy class into Java class. While this does not always produce 100% usable/runnable Java code, it is still useful for learning purposes.

* If you are using Spring Framework, know that all your beans/classes can be created as a Groovy class including things like `@Configuration`, `@Controllers`, `@Service` and other components - Groovy is a first class citizen in Spring based application.

Using this in conjunction with IntelliJ's excellent Groovy support, it is hard to understand why this not the default configuration for all Maven based Java projects.
