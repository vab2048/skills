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
- Prefer immutability over mutability.
   - Mutability for loop variables and other obvious use cases is perfectly acceptable.
   - Mutability which improves readability, is acceptable. If this is done put a comment explaining why.
- JDK9+: 
   - Use the `.of()` factory method for creating immutable collections rather than using `Collections.unmodifiable<>`
      - Specifically for Maps do not use `Map.of()` but use `Map.ofEntries()` instead. 
   - Use the `copyOf()` method for creating immutable copies of existing collections.
- JDK14+:
   - Prefer switch expressions over switch statements when possible.
- JDK15+: 
   - Use text blocks for multi-line strings. Also use text blocks when the string is a template/code snippet e.g., for JSON or SQL.
- JDK16+: 
   - When needed use instanceof pattern matching to combine a type check and cast into one step.
   - Use records when you need a "dumb data carrier"/data class rather than normal classes.
- JDK17+:
   - Where possible when creating type hierarchies, use sealed classes/interfaces to restrict which classes can extend a type and enable exhaustive switches.
   - Use algebraic data types (through sealed hierarchies) when the domain nicely fits.
      - Prefer algebraic data types (ADTs) over the visitor pattern. 
- JDK21+: 
   - Replace if-else instanceof chains with clean switch type patterns. 
   - Destructure records directly in patterns: extract fields in one step.
   - For switch: 
      - prefer exhaustive switch expressions so the compiler verifies all sealed subtypes are covered and no default case is needed.
      - add conditions to pattern cases using "when" guards to reduce boilerplate and improve readability.
      - if required, add a null case in the switch itself (only if required).
   - Collections:
      - Use the sequenced collection API additions (`getLast()`, `getFirst()`, `reversed()`)to access first/last elements and reverse views rather than raw indexes. 
      - Use the `reversed()` method on `List` when needed in loops rather than an iterator with `previous()`.
   - APIs:
      - Use the `Math.clamp()` method when needed to clamp a value between two bounds rather than using `Math.min()` and `Math.max()`. 
- JDK22+:
   - Use unnamed variables with `_` to signal intent when a variable is intentionally unused.
- JDK23+: 
   - Prefer usage of markdown (with ///) for Javadoc comments rather than the old HTML used in /** */ comments.
- JDK25+: 
   - Use flexible constructor bodies to avoid unnecessary static method creation and to validate/compute values before calling super() or this().
   - If needed you can use primitive types within pattern matching. 


## On dates and times

- Use the `java.time` package when managing dates and times.
- Where an overload for a static method exists that has a clock e.g. `Instant.now(Clock clock)` be sure to use the overload.
  - If the class does not have a field for the `Clock` then add it as a field and constructor inject it.
- Unless you specifically require a time zone offset as well, always use `Instant` for datetimes/timestamps (so we have it in UTC by default). 
- Do not use `java.util.Date` and `java.util.Calendar` unless having to interact with legacy code.
  - For new code which is written convert the legacy `Date` to an `Instant` and legacy `Calendar` to `ZonedDateTime` 
    and operate on that instead.

## On null handling

- Prefer explicit nullability contracts over defensive null checks.
- For code we own, use JSpecify:
  - Put `@NullMarked` on packages using `package-info.java`.
  - Treat unannotated types in `@NullMarked` packages as non-null.
  - Do not add `x == null` checks for values whose declared contract is non-null.
  - Use `@Nullable` only where null is part of the real API contract.
- At external trust boundaries, such as JSON deserialization, HTTP requests, database rows, environment variables, and third-party APIs:
  - Decide whether null is valid input.
  - If null is valid, mark it with `@Nullable` and handle it once at the boundary.
  - If null is invalid, reject it once at the boundary using validation or `Objects.requireNonNull`.
- Do not add speculative null checks merely because Java permits null.
- When interacting with APIs explicitly documented as nullable, handle the null case.
- When interacting with ambiguous third-party APIs, isolate the uncertainty at the call site and convert into a non-null domain value as early as practical.

## On logging

- Use the SLF4J API for logging.
- Mask any PII, secrets, passwords in log output.
- Log judiciously at DEBUG and TRACE and only at INFO when it makes sense to always output something. 

## On testing

- Use assertJ for assertions.
- Use the awaitility library for async assertions. 
   - Do not add `Thread.sleep()` in tests.

## On refactoring

- When you change existing code which contains comments, be sure to keep the comments and not lose them.
   - Also make sure (if needed) to edit the comment so it is still accurate. 

## On Spring Boot and Spring Framework usage

- Create strongly typed `@ConfigurationProperties` when needed and inject the types rather than using raw `@Value()` annotations on fields.

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

# Misc

- Always use LF line endings for .java files, even on Windows.