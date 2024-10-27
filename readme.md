# Kafka Schema Registry Spring Boot Project

This project is a Spring Boot application that integrates with Apache Kafka Confluent for producing and consuming messages serialized with Avro schemas. The project demonstrates how to use Kafka Schema Registry for managing Avro schemas and how to set up a Kafka producer and consumer with Avro serialization.

## Project Overview

- **Spring Boot Port**: `8181`
- **Kafka Confluent Platform Port**: `9021`

### Key API

- **Endpoint**: `POST http://localhost:8181/events`
  - **Description**: Produces an Avro message containing employee details to a Kafka topic.
  - **Request Body (JSON)**:
    ```json
    {
      "id": "123sfe21",
      "firstName": "Shayam",
      "lastName": "Gupta",
      "emailId": "rahul@gmail.com",
      "age": 24
    }
    ```

## Project Setup

### Prerequisites

- **Java** (version 11 or later)
- **Apache Kafka Confluent** (version 7.4.0 or compatible)
- **Schema Registry** (part of Confluent Platform)
- **Maven**

### Installation

1. **Clone the Repository**:
   ```bash
   git clone https://github.com/your-username/kafka-schema-registry-springboot.git
   cd kafka-schema-registry-springboot
   ```

2. **Build the Project**:
   Compile the project to generate the Avro classes by running:
   ```bash
   mvn clean compile
   ```

3. **Start Kafka Confluent Platform**:
   Ensure Kafka and Schema Registry are running, and accessible on port `9021` as per your configuration.

4. **Run the Application**:
   Start the Spring Boot application on port `8181`:
   ```bash
   mvn spring-boot:run
   ```

### Important Components

1. **Avro Schema (`employee.avsc`)**:
   - Schema file for `Employee` is defined in `src/main/avro/employee.avsc`.
   - After compiling, the `Employee` class is generated in the `dto` package.

2. **Maven Dependencies**:
   - Avro dependencies, plugins, and repository settings have been added in the `pom.xml` file to handle schema generation, serialization, and deserialization.

3. **Configuration**:
   - A configuration class is created to define a `newTopic` bean for the Kafka topic.
   - Producer and consumer properties, including Avro serializers and deserializers, are specified in the `application.yml` file.

4. **Service Classes**:
   - **Producer Service** (`KafkaAvroProducer`): Produces `Employee` messages to the Kafka topic.
   - **Consumer Service** (`KafkaAvroConsumer`): Consumes `Employee` messages from the Kafka topic and logs the received data.

## Example Request

Send a POST request to produce a message:

```bash
curl -X POST http://localhost:8181/events \
  -H "Content-Type: application/json" \
  -d '{
        "id": "123sfe21",
        "firstName": "Shayam",
        "lastName": "Gupta",
        "emailId": "rahul@gmail.com",
        "age": 24
      }'
```

## Application Configuration (`application.yml`)

```yaml
spring:
  kafka:
    bootstrap-servers: localhost:9092
    consumer:
      group-id: employee-group
      key-deserializer: org.apache.kafka.common.serialization.StringDeserializer
      value-deserializer: io.confluent.kafka.serializers.KafkaAvroDeserializer
      properties:
        schema.registry.url: http://localhost:9021
        specific.avro.reader: true
    producer:
      key-serializer: org.apache.kafka.common.serialization.StringSerializer
      value-serializer: io.confluent.kafka.serializers.KafkaAvroSerializer
      properties:
        schema.registry.url: http://localhost:9021
```

## Additional Information

- **Avro Serialization and Deserialization**: Uses Avro serializers and deserializers provided by Confluent to ensure consistent message formats across the producer and consumer.
- **Schema Registry**: Configured to manage Avro schemas and support schema evolution if changes are needed in future message formats.

## Dependencies

Key dependencies in `pom.xml`:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter</artifactId>
</dependency>
<dependency>
    <groupId>org.apache.avro</groupId>
    <artifactId>avro</artifactId>
    <version>1.11.1</version>
</dependency>
<dependency>
    <groupId>io.confluent</groupId>
    <artifactId>kafka-avro-serializer</artifactId>
    <version>7.4.0</version>
</dependency>
<dependency>
    <groupId>org.springframework.kafka</groupId>
    <artifactId>spring-kafka</artifactId>
</dependency>
```

## License

This project is licensed under the MIT License. See the `LICENSE` file for details.

reference: https://github.com/Java-Techie-jt/spring-kafka-avro/tree/main