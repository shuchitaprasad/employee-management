# PMD Rules Reference

This document describes the PMD rules enabled in our project. PMD is a source code analyzer that finds common programming flaws.

## Enabled Rule Categories

### 1. Best Practices
- **UnusedLocalVariable**
  - Detects declared but unused local variables
  - Reduces code clutter and improves maintainability

- **UnusedPrivateMethod**
  - Identifies private methods that are never called
  - Helps maintain clean code by removing dead code

- **UnusedFormalParameter**
  - Finds method parameters that are never used
  - Improves code clarity and API design

### 2. Code Style
- **ShortVariable**
  - Variable names should be meaningful and descriptive
  - Minimum length: 2 characters (except 'id')
  - Promotes code readability

- **LongVariable**
  - Maximum length: 30 characters
  - Ensures variable names are concise

- **MethodNamingConventions**
  - Methods should be verbs
  - Camel case naming (e.g., calculateTotal)

### 3. Design
- **CyclomaticComplexity**
  - Maximum: 10
  - Measures code complexity
  - High values indicate need for refactoring

- **NPathComplexity**
  - Maximum: 200
  - Measures number of possible execution paths
  - High values suggest overly complex methods

- **ExcessiveMethodLength**
  - Maximum: 50 lines
  - Encourages small, focused methods

### 4. Error Prone
- **EmptyCatchBlock**
  - Catches must handle or rethrow exceptions
  - Empty blocks hide errors

- **CloseResource**
  - Resources must be closed properly
  - Prevents resource leaks

- **AvoidBranchingStatementAsLastInLoop**
  - Improves loop clarity
  - Prevents hard-to-follow control flow

### 5. Performance
- **StringInstantiation**
  - Avoid `new String("literal")`
  - Use string literals directly

- **InefficientStringBuffering**
  - Detect inefficient string concatenation
  - Recommend StringBuilder usage

### 6. Security
- **HardcodedCredentials**
  - No hardcoded passwords/keys
  - Must use configuration files/environment variables

## Severity Levels

1. **Error (1)**
   - Must be fixed
   - Build fails on these
   - Example: Empty catch blocks

2. **Warning (2)**
   - Should be fixed
   - May indicate problems
   - Example: Complex methods

3. **Info (3)**
   - Style suggestions
   - Good to fix
   - Example: Variable naming

## Custom Rules Configuration

Our project customizes:
1. Cyclomatic complexity threshold (10)
2. NPath complexity threshold (200)
3. Variable naming conventions
4. Empty catch block detection

## How to Run

```bash
# Run PMD analysis
mvn pmd:check

# Generate HTML report
mvn pmd:pmd

# Skip PMD during build
mvn -Dpmd.skip=true verify
```

## Report Locations
PMD reports are generated at:
- XML: `target/pmd.xml`
- HTML: `target/site/pmd.html`

## IDE Integration
PMD rules can be integrated with:
- Eclipse
- IntelliJ IDEA
- VS Code
- NetBeans

Configure your IDE to show PMD warnings in real-time for better development experience.