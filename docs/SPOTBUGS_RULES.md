# SpotBugs Rules Reference

This document describes the SpotBugs rules enabled in our project. SpotBugs looks for potential bugs in Java code using static analysis.

## Bug Categories

### 1. Bad Practice (BAD_PRACTICE)
- Violation of "equals" or "hashCode" contracts
- Serialization errors
- Misuse of finalize()
- Ignored return values from methods
- Use of deprecated or broken methods

### 2. Correctness (CORRECTNESS)
- Invalid null pointer dereference
- Impossible casts
- Invalid method arguments
- Resource leaks
- Dead local stores
- Infinite recursive loops

### 3. Security (SECURITY)
- Unsafe deserialization
- SQL injection vulnerabilities
- XSS vulnerabilities
- Path traversal vulnerabilities
- Unsafe reflection

### 4. Malicious Code (MALICIOUS_CODE)
- Public fields that should be private
- Mutable static fields
- Arrays that should be cloned
- Exposed internal representations

### 5. Performance (PERFORMANCE)
- Inefficient use of collections
- Unread fields
- Inefficient string concatenation
- Inefficient use of streams
- Boxing/unboxing issues

### 6. Multithreaded Correctness (MT_CORRECTNESS)
- Inconsistent synchronization
- Double-checked locking
- Wait/notify misuse
- Thread pool abuse
- Volatile misuse

### 7. Dodgy Code (DODGY)
- Dead local stores
- Redundant null checks
- Unconfirmed casts
- Unchecked exception catching
- Switch fallthrough

## Priority Levels

1. **High Priority (P1)**
   - Likely to be bugs
   - Should be fixed immediately
   - Build fails on these by default

2. **Medium Priority (P2)**
   - Suspicious code
   - Should be reviewed
   - May indicate bugs

3. **Low Priority (P3)**
   - Style issues
   - Potential problems
   - Good to fix but not critical

## Custom Configuration Notes

Our project uses the following custom configurations:

1. Enforces high-priority bug detection
2. Includes FindSecBugs plugin for security checks
3. Reports are generated in XML format
4. Fails build on high-priority issues
5. Integrated with CI/CD pipeline

## How to Run Locally

```bash
# Run full analysis
mvn spotbugs:check

# Generate HTML report
mvn spotbugs:gui

# Skip SpotBugs during build
mvn -Dspotbugs.skip=true verify
```

## Report Location
SpotBugs reports are generated at:
- XML: `target/spotbugsXml.xml`
- HTML: `target/site/spotbugs.html`