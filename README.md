# DevOps Project

A Java-based Maven project that demonstrates a simple web application delivery workflow using **Maven**, **Jenkins**, **Docker**, and **Apache Tomcat**.

The repository contains:

- A small Java `Greeter` class with JUnit tests.
- A JSP-based registration page packaged as a WAR file.
- A Jenkins pipeline for checkout, build, and test stages.
- A Docker image definition for deploying the generated WAR to Tomcat.

## Technologies

- Java
- Maven
- JSP / Servlet API
- Apache Tomcat
- Docker
- Jenkins
- JUnit and Hamcrest

## Project Structure

```text
.
├── Dockerfile                         # Builds a Tomcat image containing the WAR
├── Jenkinsfile                        # Jenkins CI pipeline
├── pom.xml                            # Parent Maven project configuration
├── src/
│   ├── main/java/com/example/
│   │   └── Greeter.java               # Example Java class
│   └── test/java/com/example/
│       └── TestGreeter.java           # Unit tests for Greeter
└── webapp/
    ├── pom.xml                        # WAR module configuration
    └── src/main/webapp/
        ├── index.jsp                   # Registration page
        └── WEB-INF/web.xml             # Web application descriptor
```

## Application Overview

The web module is packaged as a WAR file named `webapp.war`. Its main page provides a basic registration form with fields for:

- Full name
- Mobile number
- Email address
- Password
- Password confirmation

The Java example module contains a `Greeter` class that returns a greeting in the following format:

```text
Hello, World!
```

## Prerequisites

Install the following tools before building the project:

- JDK 17 or a compatible Java Development Kit
- Apache Maven 3.x
- Docker, if you want to build and run the container
- Jenkins, if you want to execute the CI pipeline

Verify the local tools:

```bash
java -version
mvn -version
docker --version
```

## Build and Test

Run the Maven build from the repository root:

```bash
mvn clean package
```

Run the test suite separately:

```bash
mvn test
```

The generated WAR file is expected at:

```text
webapp/target/webapp.war
```

## Run the Web Application Locally

After packaging the application, deploy the WAR file to a local Tomcat installation:

```bash
cp webapp/target/webapp.war "$CATALINA_HOME/webapps/"
"$CATALINA_HOME/bin/catalina.sh" run
```

On Windows, copy the WAR file to `%CATALINA_HOME%\\webapps` and start Tomcat with `bin\\startup.bat`.

Once Tomcat is running, open:

```text
http://localhost:8080/webapp/
```

## Docker

The `Dockerfile` uses Apache Tomcat as the base image and copies the generated WAR file into Tomcat's `webapps` directory.

Build the application before building the image:

```bash
mvn clean package
```

Build the Docker image:

```bash
docker build -t devops-project .
```

Run the container:

```bash
docker run --rm -p 8080:8080 devops-project
```

Open the application at:

```text
http://localhost:8080/webapp/
```

## Jenkins Pipeline

The `Jenkinsfile` defines the following stages:

1. **Cleanup Workspace** – Removes files from the Jenkins workspace.
2. **Checkout from SCM** – Checks out the `main` branch from GitHub.
3. **Build Application** – Runs `mvn clean package`.
4. **Test Application** – Runs `mvn test`.

The Jenkins agent must provide:

- A node labeled `Jenkins-Agent`
- A JDK tool named `Java17`
- A Maven tool named `Maven3`
- A GitHub credential with the ID `github`

Update the credential ID, branch, repository URL, or tool names in `Jenkinsfile` if your Jenkins configuration uses different values.

## Important Configuration Notes

- The root `pom.xml` declares both `server` and `webapp` Maven modules. Ensure the `server` module exists in the checkout, or remove/update that module declaration before running Maven.
- The Docker build expects a WAR file in `webapp/target/`, so package the project before running `docker build`.
- The registration form currently uses `action_page.php` as its form action. A PHP endpoint is not included in this repository, so form submission requires a backend endpoint to be implemented or the form action to be updated.
- The project contains legacy Java and Servlet-era configuration. Review compiler, dependency, and Tomcat versions before using it in production.
- Do not use the sample registration form for real user data without adding server-side validation, secure password handling, HTTPS, CSRF protection, and persistent storage.

## Development Workflow

A typical development workflow is:

```bash
# Clone the repository
git clone https://github.com/PrachiSukreSS/DevOps_Project.git
cd DevOps_Project

# Build and test
mvn clean package
mvn test

# Build and run the container
docker build -t devops-project .
docker run --rm -p 8080:8080 devops-project
```

## License

No license is currently specified for this repository. Add a license file if you intend to distribute or reuse the project.
