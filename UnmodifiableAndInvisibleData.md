## Unmodifiable and Invisible Data

When Serializing objects, we can inspect a user's privileges
to determine the whether of not a user has access to a particular 
field such as a Social Security Number. If they do not have access
then the field can be encrypted. If they have read-only privileges
the data can be digitally signed.

If the caller eventually asks to update the object, the privileges
can be checked when deserializing the object. If the encrypted or 
signed data has changed and the user does not have access then the 
update can be rejected.

This information can be leveraged dynamically on the front-end.
For example, Data Entry fields can be hidden for fields which are 
encrypted or made read-only for data which is digitally signed. 


### Example

Consider the following structure:

    struct Person { first_name: string }

we can augment this structure with metadata to indicate that a Primary Key should be encrypted.

_NOTE_: while the syntax below may be similar to Rust macros it is not.

    struct Person {
        #[encrypt(normal_user)]
        #[read_only(special_user)]
        #[read_write(super_special_user)]
        first_name: string,
    }
