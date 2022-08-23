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



