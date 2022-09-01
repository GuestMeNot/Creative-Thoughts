### Opinionated Data Queries

[WIP - currently this is very simplified. There is much more functionality to be added]

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


1. When we deserialize the following JSon-like structure into a person:

        struct Person {
          #['=']
          first_name: string,
        }

        let x = { }
        let person = Person::deserialize(x);
        let query = person.to_sql();

   Where `query` has the following value:

        SELECT DISTINCT * FROM Person

2. When we deserialize the following JSon-like structure into a person.

        struct Person {
          #['=']
          first_name: string,
        }

        let x = { "first_name": "Hello" }
        let person = Person::deserialize(x);
        let query = person.to_sql();

   Where `query` has the following value:

        SELECT DISTINCT * FROM Person WHERE first_name = 'Hello'

3. We can create like clauses. 

        struct Person {
          #['like']
          first_name: string,
        }

        let x = { "first_name": "H" }
        let person = Person::deserialize(x);
        let query = person.to_sql();

   Where `query` has the following value:

        SELECT DISTINCT * FROM Person WHERE first_name like 'H*'

4. We can query over two fields:

        struct Person {
          #['like']
          first_name: string,
          is_human: bool,
        }

        let x = { "first_name": "H", "is_human": "true" }
        let person = Person::deserialize(x);
        let query = person.to_sql();

   Where `query` has the following value:

        SELECT DISTINCT * FROM Person 
        WHERE first_name like 'H*' AND is_human = true

5. We can query over 1 of two fields:

        struct Person {
          #['like']
          first_name: string,
          is_human: bool,
        }

        let x = { "first_name": "H", "is_human": "true" }
        let person = Person::deserialize(x);
        let query = person.to_sql();

     Where `query` has the following value:

        SELECT DISTINCT * FROM Person 
        WHERE first_name like 'H*' AND is_human = true

6. A struct and a database table can have different names:

         #[table(People)]
         struct Person {
         }

         let x = {  }
         let person = Person::deserialize(x);
         let query = person.to_sql();

   Where `query` has the following value:

        SELECT DISTINCT * FROM People 

7. A field in the struct and a database table can have different names:

        struct Person {
          #['=']
          #[field(first)]
          first_name: string,
        }

        let x = { "first_name": "Hello" }
        let person = Person::deserialize(x);
        let query = person.to_sql();

   Where `query` has the following value:

        SELECT DISTINCT * FROM Person WHERE first = 'Hello'

8. A field can have a `IN` clause:

        struct Person {
          #['IN']
          first_name: string,
        }

        let x = { "first_name": ["Hello", "World"] }
        let person = Person::deserialize(x);
        let query = person.to_sql();

   Where `query` has the following value:

        SELECT DISTINCT * FROM Person
        WHERE first IN ('Hello', 'World')

9. Queries can be across table associations:

        struct Person {
          #[key]
          id: int,
          address: Address,
        }

        struct Address {
          id: int,
          #['=']
          postal_code: string,
          #[key(Person)]
          person_id: int,   
        }

        let x = { "address": {postal_code: "1234"} }
        let person = Person::deserialize(x);
        let query = person.to_sql();

    Where `query` has the following value:

        SELECT DISTINCT * FROM Person p, Address a
        WHERE p.id = a.person_id AND a.postal_Code = '1234'

    NOTE: Aliases `p` and `a` would need to be generated and unique. 
