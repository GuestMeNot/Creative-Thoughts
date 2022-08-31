### Opinionated Data Queries

In CRUD applications many queries can be represented by metadata. If the metadata value
is present, the a [Search Argument or (SARG)](https://www.sqlconsulting.com/archives/understanding-search-arguments/) 
can be generated and attached to a database query.

We can make the following simplifications for typical CRUD operations:

- return all column values for a query. Since most network cost needed to the results are 
  reasonably small in cost compared to the wait time spent in the database engine to 
  parse a query and return the returns from [SSD](https://en.wikipedia.org/wiki/Solid-state_drive).  
- results should typically be limited in size. We can add a `row limit` to ensure this.
- Only binary operators: `=`, `>`, `<`, `!=`, `IN`, etc.
- Only `AND` conditions. `OR` requires more complex groupings.
- Additional query clauses can be added to support more complex situations `OR`, `EXISTS`, etc.
- Use only standard SQL.

For Example consider the following structure:

    struct Person { first_name: string }

we can augment this structure with metadata to indicate a Table and column to query along with an operator.

_NOTE_: while the syntax below may be similar to Rust macros it is not. Also, values are
assumed to be `null` or `nil` unless explicitly provided.

    struct Person { 
        #['=']
        first_name: string,
    }

1. When we deserialize the following JSon-like structure into a person:

        let x = { }
        let person = Person::deserialize(x);
        let query = person.to_sql();

   We would like `query` to have the following value:

        SELECT DISTINCT * FROM Person

2. When we deserialize the following JSon-like structure into a person.

        let x = { "first_name": "Hello" }
        let person = Person::deserialize(x);
        let query = person.to_sql();

    We would like `query` to have the following value:

        SELECT DISTINCT * FROM Person WHERE first_name = 'Hello'

3. 