
### A few random thoughts and architectural ideas.

Toward [DRYer](https://medium.com/kite-srm/what-writing-dry-code-really-means-a8bb031289c9) 
code from an architectural perspective:

- [Easy Error Handling](EasyErrorHandling.md) ensures multiple stakeholders receive the information
  they need in a timely manner without additional noise.

- [Simplified Field Validation](Validation.md) keeps field validation code in sync between 
  the Browser code and server code.

- [Unmodifiable Data and Invisible Data](UnmodifiableAndInvisibleData.md) can be used to 
  either hide data from a user while making updates simpler. For example, a person has an 
  [SSN](https://www.ssa.gov/ssnumber/) which for security reasons should only be visible to a 
  limited set of users. To update a person's data, the SSN should be included in the update 
  but should be invisible to users who do not have the proper permissions. Likewise, there are 
  pieces of data which should be visible to a user such as 
  [Latitude and Longitude](https://www.timeanddate.com/geography/longitude-latitude.html) of a Geolocation, 
  but which should never be updated. Writing custom code for each of these situations is redundant.
  This should be checked on the server but also handled on the front-end so that components
  can automatically detect what is possible with a particular piece of data.

- [URL Signing](UrlSigning.md) guarantees that only authorized users can update data fields 
  and disables or hides buttons for which the user does not have Authorization.

- [Primary Key Encryption](PrimaryKeyEncryption.md) ensures that Rest Get requests for a single
  data item does not reveal its key to attackers. Suppose PKs are sequential.
  An attacker can guess the next several numbers in a sequence to gain access to data
  they would normally not be allowed to see. Signed PKs can be used to link data from
  one location to another. For example, suppose we want to assign a State to an Address.
  
- [Opinionated Data Queries](OpinionatedDataQueries.md) simplify the process of creating
  data queries. Rather than writing a plethora of custom code to create simple queries
  we can dynamically build queries on the server in a way that does not create security 
  holes. We can also leverage [Unmodifiable Data and Invisible Data](UnmodifiableAndInvisibleData.md)
  to limit which query fields a user has access to.

- [Code Generation using Templates](CodeGeneration.md) allows boilerplate code to be
  generated.  

Improving User Experiences:

- [Opinionated Data Field Lifecycle States](OpinionatedDataFieldLifecycle.md) visually informs uses of
  the state the data is in at a glance.

- [Reloading Data State of an Application](Reload.md) brings the user back to
  the state they were in when the application last exited.  

Security

- In a typical enterprise, verifying that a user has a specific role means looking
  through several roles. These roles often come from a centralized location such as 
  an LDAP server. There are often too many roles to add to a 
  [JWE](https://medium.com/javarevisited/what-are-jwt-tokens-and-their-different-forms-jws-and-jwe-bea92e61a6c2)
  token in a stateless environment. Applications generally allow access to a relatively 
  small number of roles. The set of roles allowed for an application can be read from
  metadata attached to each Rest method.

### Distributed Mutex

Here are a few thoughts around creating a [Distributed Mutex](DistributedMutex.md).