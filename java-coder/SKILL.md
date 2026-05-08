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

## On style

- Do not add the `final` keyword to arguments which are already effectively final.
  - Only add the `final` keyword to arguments if there is a good reason. 
- Never return null from a method. If an indication of absense is needed return an `Optional`.
- Do not create interfaces when there will only be a single implementation.
  - If other implementation(s) are required for tests then use mocking rather than creating the interface.
- Where you need a dumb data carrier use records.
- JDK14+: Use algebraic data types (through sealed hierarchies) when the domain nicely fits. 
  - Prefer algebraic data types (ADTs) over the visitor pattern.

## On dates and times

- Use the `java.time` package when managing dates and times.
- Where an overload for a static method exists that has a clock e.g. `Instant.now(Clock clock)` be sure to use the overload.
  - If the class does not have a field for the `Clock` then add it as a field and constructor inject it.
- Unless you specifically require a time zone offset as well, always use `Instant` for datetimes/timestamps (so we have it in UTC by default). 
- Do not use `java.util.Date` and `java.util.Calendar` unless having to interact with legacy code.
  - For new code which is written convert the legacy `Date` to an `Instant` and legacy `Calendar` to `ZonedDateTime` 
    and operate on that instead.

## On null handling

- When interacting with APIs which explicitly mention that they can return null:
  - Ensure the null case is handled appropriately.
- When interacting with ambiguous APIs:
  - Assume the API can return null. 
- On code we are writing:
  - Use the jSpecify dependency annotations to embrace a non-null by default. 
  - Mark using annotations where null can be given.  

## On logging

- Use the SLF4J API for logging.
- Mask any PII, secrets, passwords in log output.
- Log judiciously at DEBUG and TRACE and only at INFO when it makes sense to always output something. 

## On testing


# Dependency Management 

- When a dependency is added into a build file add a comment explaining why this dependency is needed, i.e., what functionality it provides for the app.
- On BOM usage:
  - Use the latest versions of the BOMs which are stated to be compatible with each other.
  - Elide specific versions for dependencies contained within the BOM. 
- Spring Boot BOMs: 
  - org.springframework.boot:spring-boot-dependencies
  - org.springframework.cloud:spring-cloud-dependencies
  - io.awspring.cloud:spring-cloud-aws-dependencies
- AWS BOMs:
  - software.amazon.awssdk:bom 
  - software.amazon.awscdk:bom