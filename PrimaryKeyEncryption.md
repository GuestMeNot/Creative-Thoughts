## Primary Key Encryption

Attackers will often try to discover primary key generation schemes
to attempt to gain access to additional data.

When Serializing Objects, database primary keys and foreign keys can be 
marked with metadata which allows database specific keys to be encrypted. 

On the front-end, when making an association between two objects, the 
encrypted key is assigned since it will be decrypted properly on the server
when the data is saved.

Front-End components can be made aware of this assignment so that the result
is seamless to the developer and front-end user.

### Example

Consider the following structure:

    struct Person { id: int; first_name: string }

we can augment this structure with metadata to indicate that a Primary Key should be encrypted.

_NOTE_: while the syntax below may be similar to Rust macros it is not.

    struct Person {
        #[encrypt]
        id: int
        first_name: string,
    }
