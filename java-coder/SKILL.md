---
name: java-coder
description: >-
  Java coder for us with any Java or JVM coding task: writing, editing, refactoring, reviewing,
  debugging, testing, or explaining Java code; working with Spring, Maven, Gradle,
  JUnit, Mockito, packages, modules, generics, annotations, records, exceptions,
  concurrency, dependency injection, serialization, or Java build and quality
  tooling.
version: 0.0.1
author: vab2048
license: GNU GPLv3
---

# When to Use

Use this skill when the user wants to:
- Write, edit, refactor, debug, or explain Java code
- Review Java code for correctness, maintainability, performance, or style
- Add, update, or troubleshoot Java tests, including JUnit, Mockito, AssertJ, or integration tests
- Work with JVM project structure, packages, modules, annotations, records, enums, interfaces, or generics
- Modify Java build or quality tooling, including Maven, Gradle, compiler settings, Checkstyle, Spotless, PMD, Error Prone, or static analysis configuration
- Work on Java frameworks or libraries, including Spring, Spring Boot, Jakarta EE, Hibernate, JPA, Micronaut, Quarkus, or similar JVM application code
- Diagnose Java runtime behavior, exceptions, logging, concurrency, resource management, serialization, or dependency injection issues
- Make decisions where Java language level, API compatibility, nullability, immutability, collections, or exception strategy matter

# Coding Style

- Do not add the `final` keyword to arguments which are already effectively final.
  - Only add the `final` keyword to arguments if there is a good reason. 
- On dates and times:
  - Use the `java.time` package when managing dates and times.
  - Where an overload for a static method exists that has a clock e.g. `Instant.now(Clock clock)` be sure to use the overload.
    - If the class does not have a field for the clock then add it as a field.  
  - Do not use `java.util.Date` and `java.util.Calendar` unless having to interact with legacy code.
    - For new code which is written convert the legacy `Date` to an `Instant` and legacy `Calendar` to `ZonedDateTime` 
      and operate on that instead.
