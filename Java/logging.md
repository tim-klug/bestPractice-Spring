# Logging

Standard logging with SLF4J logger.

## normal class

```java
private final Logger logger = LogManager.getLogger(this.class);
```

## static class

```java
private static final Logger logger = LogManager.getLogger(ActualClass.getClass());
```