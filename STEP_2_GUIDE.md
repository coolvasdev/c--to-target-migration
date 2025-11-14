# Migration Step 2: Setup Target Environment

## **Overview and Objectives**

The goal of this step is to set up a fully functional Java development environment to accommodate the migration of your application from C# to Java. This includes installing the necessary tools, configuring the project structure, and preparing the build system to ensure a smooth transition and development experience.

### **Objectives**
- Install and configure the Java development environment.
- Set up a Java project structure compatible with your application needs.
- Configure build tools to manage dependencies, build, and run the application.

---

## **Prerequisites and Dependencies**
Before starting, ensure the following:

1. **Repository Access**: You have access to the `repository` where the C# code is stored.
2. **Java Development Tools**:
   - JDK 11 or higher (recommended: JDK 17 for long-term support).
   - An IDE for Java development (e.g., IntelliJ IDEA, Eclipse, or VS Code with Java extensions).
   - Build tool: Maven or Gradle.
3. **Environment Setup**:
   - Admin access to install required software.
   - Ensure disk space for dependencies and compiled files.
4. **Basic Knowledge**: Familiarity with Java syntax, project structure, and build tools.
5. **C# Knowledge**: Understanding of the C# application's structure and functionality.

---

## **Step-by-Step Implementation Instructions**

### **1. Install the Java Development Environment**

#### **Install Java JDK**
1. Download and install the latest JDK:
   - Oracle JDK: [https://www.oracle.com/java/technologies/javase-downloads.html](https://www.oracle.com/java/technologies/javase-downloads.html)
   - OpenJDK: [https://openjdk.org/install/](https://openjdk.org/install/)
2. Verify installation:
   ```bash
   java -version
   # Output should show the installed Java version (e.g., Java 17).
   ```
3. Set the `JAVA_HOME` environment variable:
   - On Linux/Mac:
     ```bash
     export JAVA_HOME=/path/to/jdk
     export PATH=$JAVA_HOME/bin:$PATH
     ```
   - On Windows:
     - Add `JAVA_HOME` to the system environment variables.
     - Append `%JAVA_HOME%\bin` to the PATH variable.

#### **Install an IDE**
- Download and install your preferred IDE:
  - IntelliJ IDEA: [https://www.jetbrains.com/idea/](https://www.jetbrains.com/idea/)
  - Eclipse: [https://www.eclipse.org/downloads/](https://www.eclipse.org/downloads/)
  - Visual Studio Code (Java extensions): [https://code.visualstudio.com/](https://code.visualstudio.com/)

#### **Install Build Tools**
- **Maven** (if using Maven):
  - Install Maven: [https://maven.apache.org/download.cgi](https://maven.apache.org/download.cgi)
  - Verify installation:
    ```bash
    mvn -version
    # Output should show the installed Maven version.
    ```
- **Gradle** (if using Gradle):
  - Install Gradle: [https://gradle.org/releases/](https://gradle.org/releases/)
  - Verify installation:
    ```bash
    gradle -v
    # Output should show the installed Gradle version.
    ```

---

### **2. Set Up the Project Structure**

#### **Standard Java Project Structure**
Follow the Maven or Gradle convention for project structure:
```
project-root/
├── src/
│   ├── main/
│   │   ├── java/           # Java source code
│   │   ├── resources/      # Configuration files
│   ├── test/
│       ├── java/           # Unit tests
│       ├── resources/      # Test resources
├── pom.xml (for Maven) or build.gradle (for Gradle)
```

#### **Create the Project**
1. **Using IntelliJ IDEA**:
   - File > New > Project > Maven or Gradle.
   - Name the project and set the location.
   - Select JDK version and build tool.

2. **Using Command Line**:
   - For Maven:
     ```bash
     mvn archetype:generate -DgroupId=com.example.app -DartifactId=myapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
     ```
   - For Gradle:
     ```bash
     gradle init
     # Select "application" when prompted.
     ```

3. **Manually Create Directories**:
   - If you prefer manual setup, create the `src/main/java`, `src/main/resources`, and `src/test/java` directories.

---

### **3. Configure Build Tools**

#### **Maven Configuration (pom.xml)**
Add dependencies and plugins to the `pom.xml` file:
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.example.app</groupId>
    <artifactId>myapp</artifactId>
    <version>1.0-SNAPSHOT</version>
    <dependencies>
        <!-- Example: Add dependencies here -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
            <version>3.1.0</version>
        </dependency>
    </dependencies>
</project>
```

#### **Gradle Configuration (build.gradle)**
Add dependencies and tasks in the `build.gradle` file:
```groovy
plugins {
    id 'java'
}

group = 'com.example.app'
version = '1.0-SNAPSHOT'

repositories {
    mavenCentral()
}

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter:3.1.0'
    testImplementation 'org.junit.jupiter:junit-jupiter:5.9.0'
}
```

---

### **Code Example**
A simple "Hello, World!" application in Java:
```java
package com.example.app;

public class Main {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```

Place this file in `src/main/java/com/example/app/Main.java`.

---

## **Common Pitfalls and How to Avoid Them**
1. **Incorrect JDK Version**:
   - Ensure you are using JDK 11 or higher.
   - Verify the `JAVA_HOME` configuration.
2. **Build Tool Conflicts**:
   - Use either Maven or Gradle, not both.
   - Ensure compatible dependencies.
3. **Incorrect Project Structure**:
   - Follow the standard directory layout for Java projects.

---

## **Testing Checklist for This Step**
1. Verify Java installation:
   ```bash
   java -version
   ```
2. Verify the build tool:
   - For Maven:
     ```bash
     mvn compile
     ```
   - For Gradle:
     ```bash
     gradle build
     ```
3. Compile and run the "Hello, World!" application:
   ```bash
   java -cp target/classes com.example.app.Main
   ```

---

## **Validation Criteria**
- The Java development environment is properly installed and configured.
- The project structure adheres to Java conventions.
- The build tool successfully compiles and runs a basic Java program.

---

## **Troubleshooting Guide**
1. **Java not recognized**:
   - Check if `JAVA_HOME` is set correctly.
   - Ensure the PATH includes `JAVA_HOME/bin`.
2. **Build tool errors**:
   - Ensure dependencies are correctly specified in `pom.xml` or `build.gradle`.
   - Run `mvn clean` or `gradle clean` to resolve cache issues.

---

## **Resources and References**
- [Official Java Documentation](https://docs.oracle.com/en/java/)
- [Maven Documentation](https://maven.apache.org/guides/index.html)
- [Gradle Documentation](https://docs.gradle.org/current/userguide/userguide.html)
- [Java Standard Project Structure](https://maven.apache.org/guides/introduction/introduction-to-the-standard-directory-layout.html)

---

## **Next Steps**
- Proceed to **Step 3: Translate Core Components**.
- Begin migrating core application logic from C# to Java.

Estimated Time: 2-3 hours, depending on familiarity with tools.