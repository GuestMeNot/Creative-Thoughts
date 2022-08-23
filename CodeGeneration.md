### Code Generation using Templates

Much of the code in a typical suite of Microservices is boilerplate.
For example, we may need to create a Person service 

Rewriting this code repeatedly is a waste of time. Generics help 
but are not a complete solution as in a large application there are 
several files which need to be kept in sync. 

In many applications code, including database model objects could be generated
just using a database structure. Several RMDBMs provide access to the
database structure, which can be leveraged to generate the model objects,
[Data Access Objects](https://en.wikipedia.org/wiki/Data_access_object) 
and Rest Microservices.

Additionally, if signatures  or code changes then the application code 
must be modified in several locations. We could just update a template
and regenerate the code. 

These templates could be placed in a hierarchical directory structure
and the generated code can be placed in this directory structure
so that all applications start with a common scaffolding. 

