# Serialization 

## 1. Formal Definition
**Serialization** is:
> The process of converting an in-memory object into a **byte stream** or **text representation** that can be stored or transmitted and later reconstructed (deserialized).

Formats include:
- JSON
- XML
- Protocol Buffers
- Avro
- BSON
- Binary proprietary formats

---

## 2. Practical Meaning in Software Engineering

Serialization is used when:
- sending data over a network (APIs, microservices)  
- storing objects in files or caches  
- messaging (Kafka, RabbitMQ, SQS)  
- persisting session state  
- logging structured data  
- RPC (gRPC, Thrift, etc.)  

Example (Python JSON):

```python
import json

user = {"id": 1, "name": "Alice"}
data = json.dumps(user)      # serialization (object → string)
restored = json.loads(data)  # deserialization
```

---

## 3. Key Properties

- **Compactness** (size)
- **Speed** (encode/decode)
- **Schema** (strict vs flexible)
- **Compatibility** (language/platform support)
- **Human readability** (JSON vs binary)
- **Extensibility** (evolution over time)

---

## 4. Common Serialization Formats

### 4.1 JSON
- human-readable
- widely supported
- text-based
- great for web APIs

### 4.2 Protocol Buffers (Protobuf)
- binary, compact, fast
- requires schema (`.proto` files)
- strong typing
- great for internal microservices

### 4.3 XML
- verbose, structured
- supports attributes, namespaces
- common in older enterprise systems

### 4.4 Custom Binary Formats
- very efficient when tuned
- but higher maintenance and coupling

---

## 5. Versioning and Schema Evolution

Big challenge:
> How to change object structure without breaking old data?

Strategies:
- use optional fields  
- provide default values  
- keep backward compatibility (old readers handle new data)  
- keep forward compatibility (new readers handle old data)  

Example in JSON:
- New field added → old readers simply ignore it.

In Protobuf:
- fields have numeric IDs; avoid reusing them.  

---

## 6. Serialization in Databases and Caches

- Caching systems (Redis, Memcached) store serialized data  
- ORMs sometimes serialize blobs (e.g., JSON column)  
- Session stores hold serialized user data  

---

## 7. Security Concerns (Important)

Serialization can be dangerous when:

- accepting serialized data from **untrusted sources**
- using **language-native serializers** (Java, Python `pickle`) that can execute code during deserialization

Typical vulnerability:
- **Insecure deserialization → Remote Code Execution (RCE)**

Safe practices:
- Don’t deserialize untrusted binary objects  
- Use safe formats (JSON, Protobuf)  
- Validate data before use  
- Avoid `pickle` or native object serialization for untrusted input  

---

## 8. Interview Questions

1. What is serialization and why is it needed?  
2. Difference between JSON and Protobuf?  
3. What is schema evolution?  
4. What security issues exist with serialization?  
5. How do you design data contracts for backward compatibility?  
6. How is serialization used in microservices?  

---

## 9. Winning Interview Answer Template

> “Serialization converts in-memory objects into a format that can be stored or transmitted and later reconstructed.  
I typically use JSON for external APIs and binary formats like Protobuf for internal services.  
I pay attention to versioning, schema evolution, and security — especially avoiding insecure deserialization of untrusted input.”

---

## 10. One-Minute Summary

- Serialization = object → transferable representation  
- Used in networking, storage, caching, messaging  
- JSON/Protobuf most common today  
- Versioning and security are major concerns  
