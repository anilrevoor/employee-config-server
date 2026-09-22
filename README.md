# Employee Config Server

## Overview

Employee Config Server is a Spring Cloud Config Server used to provide centralized configuration for the Employee Management application.

It separates application configuration from application code and allows environment-specific configuration to be managed centrally.

## Technology Stack

* Java 21
* Spring Boot
* Spring Cloud Config Server
* Maven

## Application URL

Config Server:

`http://localhost:8888`

## Configuration Repository

The configuration repository contains:

```text
employee-management-dev.properties
```

This configuration belongs to:

```text
Application: employee-management
Profile: dev
```

## Example Configuration

```properties
app.environment=development
logging.level.root=DEBUG
spring.jpa.show-sql=true
```

## Verify Configuration

Open:

`http://localhost:8888/employee-management/dev`

The Config Server should return the configuration associated with:

```text
Application = employee-management
Profile = dev
```

## Backend Configuration

The Employee Management backend uses:

```properties
spring.application.name=employee-management
spring.profiles.active=dev
```

The backend therefore requests configuration for:

```text
employee-management + dev
```

## Architecture

```text
+----------------------------+
| Employee Management        |
| Spring Boot Backend :8080  |
+-------------+--------------+
              |
              | Configuration
              v
+----------------------------+
| Spring Cloud Config Server |
| Port :8888                 |
+-------------+--------------+
              |
              v
+----------------------------+
| Configuration Repository   |
| employee-management-dev    |
| .properties                |
+----------------------------+
```

## Purpose

Spring Cloud Config provides:

* Centralized configuration
* Environment-specific profiles
* Separation of configuration from code
* Easier configuration management
* Consistent configuration across environments

## Profile

Development profile:

`dev`

Configuration file:

`employee-management-dev.properties`

## Startup

Start the Config Server before starting the Employee Management backend.

Recommended order:

```text
1. Config Server :8888
2. Employee Management Backend :8080
3. Gateway :8081
4. React Frontend :3000
```

## Verification

After starting the Config Server, verify:

```text
http://localhost:8888/employee-management/dev
```

Then start the backend and confirm that it can obtain the centralized configuration.

## Troubleshooting

### Config Server is not reachable

Check:

```text
http://localhost:8888
```

Make sure the Config Server is running.

### Backend cannot load configuration

Check:

* Config Server is running
* Backend application name is `employee-management`
* Active profile is `dev`
* `employee-management-dev.properties` exists
* Config Server is running on port `8888`

## Complete Architecture

```text
                   +----------------------+
                   | React Frontend       |
                   | :3000                |
                   +----------+-----------+
                              |
                              v
                   +----------------------+
                   | Gateway              |
                   | :8081                |
                   +----------+-----------+
                              |
                              v
                   +----------------------+
                   | Backend              |
                   | :8080                |
                   +----------------------+
                              |
                              v
                         H2 Database


                   +----------------------+
                   | Config Server        |
                   | :8888                |
                   +----------+-----------+
                              |
                              v
                   Configuration Repository
```
