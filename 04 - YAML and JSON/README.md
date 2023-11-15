# YAML & JSON
- **JSON (JavaScript Object Notation)** is a lightweight data interchange format that is easy for humans to read and write and easy for machines to parse and generate.
- **YAML, which stands for "YAML Ain't Markup Language"** (a playful recursive acronym), is a human-readable data serialization format. It is often used for configuration files and data exchange between languages with different data structures. YAML uses indentation and a simple syntax to represent data hierarchically.

JSON:
```
{
  "name": "John Doe",
  "age": 30,
  "email": "john.doe@example.com",
  "address": {
    "street": "123 Main Street",
    "city": "Anytown",
    "state": "CA",
    "zip": "12345"
  },
  "hobbies": ["reading", "hiking", "photography"],
  "isStudent": false
}

```
YAML:
```
name: John Doe
age: 30
email: john.doe@example.com
address:
  street: 123 Main Street
  city: Anytown
  state: CA
  zip: 12345
hobbies:
  - reading
  - hiking
  - photography
isStudent: false

````