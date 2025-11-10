# Error Prone Rules Reference

This document describes the Error Prone rules enabled in our project. Error Prone is a static analysis tool from Google that catches common Java programming mistakes at compile time.

## Core Rules Categories

### 1. Likely Errors

#### ArrayEquals
- **Severity**: ERROR
- **Description**: Using `equals()` to compare arrays instead of `Arrays.equals()`
- **Example**:
```java
// Wrong
if (array1.equals(array2)) { ... }

// Correct
if (Arrays.equals(array1, array2)) { ... }
```

#### ArrayToString
- **Severity**: WARNING
- **Description**: Calling `toString()` on an array returns its identity rather than contents
- **Example**:
```java
// Wrong
logger.info("Array: " + myArray.toString());

// Correct
logger.info("Array: " + Arrays.toString(myArray));
```

### 2. Best Practices

#### CatchFail
- **Severity**: ERROR
- **Description**: fail() in catch block swallows exception
- **Example**:
```java
// Wrong
try {
    something();
} catch (Exception e) {
    fail(); // Original exception is lost
}

// Correct
try {
    something();
} catch (Exception e) {
    fail("Failed due to: " + e.getMessage());
}
```

#### ClassCanBeStatic
- **Severity**: WARNING
- **Description**: Inner classes that don't reference enclosing class should be static
- **Example**:
```java
class Outer {
    // Wrong
    class Inner {
        void doWork() {}
    }

    // Correct
    static class StaticInner {
        void doWork() {}
    }
}
```

### 3. Immutability

#### ImmutableEnumChecker
- **Severity**: WARNING
- **Description**: Enums should be immutable
- **Example**:
```java
// Wrong
enum Status {
    ACTIVE, INACTIVE;
    List<String> notes = new ArrayList<>();  // Mutable field
}

// Correct
enum Status {
    ACTIVE, INACTIVE;
    private final List<String> notes = List.of();  // Immutable field
}
```

### 4. String Handling

#### StringEquality
- **Severity**: ERROR
- **Description**: Using == to compare strings
- **Example**:
```java
// Wrong
if (str == "value") { ... }

// Correct
if (str.equals("value")) { ... }
```

### 5. Exception Handling

#### CatchAndPrintStackTrace
- **Severity**: WARNING
- **Description**: Avoid printStackTrace(); use proper logging
- **Example**:
```java
// Wrong
try {
    doSomething();
} catch (Exception e) {
    e.printStackTrace();
}

// Correct
try {
    doSomething();
} catch (Exception e) {
    logger.error("Operation failed", e);
}
```

## How to Run

### Command Line
```bash
# Compile with Error Prone enabled
javac -XDcompilePolicy=byfile -J-Xmx2g \
      -processorpath error_prone_core-2.10.0.jar \
      -Xplugin:ErrorProne MyFile.java
```

### Maven Configuration
```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <version>3.8.1</version>
    <configuration>
        <compilerArgs>
            <arg>-XDcompilePolicy=byfile</arg>
        </compilerArgs>
        <annotationProcessorPaths>
            <path>
                <groupId>com.google.errorprone</groupId>
                <artifactId>error_prone_core</artifactId>
                <version>2.10.0</version>
            </path>
        </annotationProcessorPaths>
    </configuration>
</plugin>
```

## Suppressing Warnings

### Method Level
```java
@SuppressWarnings("ArrayEquals")
public void method() {
    // code
}
```

### Statement Level
```java
@SuppressWarnings("StringEquality")
if (str == "value") { // suppressed warning
    // code
}
```

## Severity Levels

1. **ERROR**
   - Must be fixed
   - Compilation fails
   - Example: StringEquality

2. **WARNING**
   - Should be fixed
   - Build succeeds but warns
   - Example: ClassCanBeStatic

## IDE Integration

Error Prone works with:
- IntelliJ IDEA (native support)
- Eclipse (plugin required)
- VS Code (via Java extension)
- NetBeans (via plugin)

## Best Practices

1. Run Error Prone checks before committing
2. Fix ERROR severity issues immediately
3. Review WARNING severity periodically
4. Document suppressions with clear justification
5. Include in CI/CD pipeline

## Report Location
Error Prone issues appear directly in:
- Compiler output
- IDE warnings
- Build logs

## Note on Performance
Error Prone is integrated into the compilation process, so it:
- Adds minimal build time overhead
- Catches issues early in development
- Prevents bugs from reaching production