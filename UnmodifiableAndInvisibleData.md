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
        #[encrypt]
        #[read_only(special_user_role)]
        #[read_write(super_special_user_role)]
        first_name: string,
    }

In this example, users who have `special_user_role` would get read_only access to the data,
user with `super_special_user_role` would get read and write access to the data,
and all other users would see encrypted data.

Extending this approach we could inject a list of roles so that this does not become a maintenance point:

    struct Person {
        #[encrypt]
        #[read_only('$read_only_user_roles')]
        #[read_write('$read_write_user_roles')]
        first_name: string,
    }
