### Keeping browser and server validation logic in sync is tricky.

Data validation logic may span several teams, requiring coordination and communication.
To mitigate this problem code can be written so that it automatically keeps the browser and 
server data validation in sync with database model objects. 

Java Validation can be attached to data fields of these objects to be sent to the database. 
In Java, [JPA Validation](https://beanvalidation.org/1.0/spec/) can be added to model objects. These Validation annotations 
can be read and a [JSON Schema](http://json-schema.org/specification.html) 
generated from them to be sent to the browser. 

In _Rust_, [procedural macros](https://doc.rust-lang.org/reference/procedural-macros.html) 
can generate both a json_schema() method and a validate() method for the object during 
compilation.

The generated JSON Schema can then be sent to caller.

In both cases Front-End Components in React can be hooked directly to the
JSON Schema and validation methods can be attached to fields.

For example consider the following:

    #[validation]
    #[description(A Human)]
    struct Person {
      #[description(Human First Name)]
      #[regex([A-Za-z]*]
      #[required]
      first_name: string,
    }

We could write the following:

    let x = { "first_name": "1234" }
    let person = Person::deserialize(x);
    let valid = person.is_valid();
    let schema = Person::schema();

In this example:

    valid == false

and `schema` variable above would be set to:

    {
      "$schema": "https://json-schema.org/draft/2020-12/schema",
      "$id": "ipfs://.../person.schema.json",
      "title": "Person",
      "description": "A Human",
      "type": "object",
      "properties": {
        "first_name": {
        "description": "Human First Name",
        "type": "string"
      }
      required: [ "first_name" ]
    }

### NOTE

- There are several validation mechanisms, libraries, etc. which already 
  exist in several languages which can be leveraged for validation.
- The return value from `is_valid()` above would likely make more sense as
  a struct.

