## URL Signing

Attackers like to gain access to HTML, grab the URLs and start attacking
a server to retrieve information. This can be done on the machine of an 
unsuspecting victim, or from another machine. 
[Signed URLs](https://cloud.google.com/cdn/docs/using-signed-urls) prevent
this type of attack.

Signed URLs can also prevent a colleague from accessing data which they do not 
have permission to because the digital signature is tied to a specific user.

Since a set of Signed URLs describe what functionality a user has access to
they can also be used to turn functionality on or off in a client. For example,
if a user can't delete a piece of data then they will not have the DELETE signed
URL and a DELETE button can be grayed out, disabled or invisible.

Signed URLs can be sent to a caller so that a caller knows which
[Rest](https://en.wikipedia.org/wiki/Representational_state_transfer) URLs they can call.
Suppose a Rest Service has the following endpoint [semantics](https://en.wikipedia.org/wiki/Representational_state_transfer#Semantics_of_HTTP_methods):
 
- Get a List of Data via a [Post](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/POST) using query parameters.  
- Get a single data item via a [Get](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/GET).
- Update a single data item via a [Put](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/PUT)
- Delete a single Data item via a [Delete](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/DELETE)

This set of URLs can be signed and sent to the prospective calling client.
When the calling client calls a Rest call it sends to URL, together with the
Signature, this can be checked on the server against a 
[URL Template](https://www.rfc-editor.org/rfc/rfc6570) (so that parameter substitution
does not affect the URL Signature). 

Signed URLs can be created for a caller, by looking up metadata associated with a 
Rest Service which gives Access control to the service as well as the URL template
data associated with the service. This builds an access control mechanism
for the calling client.

When a Rest URL is called by the client, the URL template is used to
build the actual URL, and calling the URL involves sending the URL Signature
back to the server. Since the signature is tied to a caller, an attacker who
intercepts to call cannot use the intercepted traffic to gain access to data.

When a Rest call is received by The Rest Server which signed the URL, 
the Server knows it signed the URL and for whom the URL was signed, so it can grant 
access a URL if the User has the Authorization to call the URL. 

Front-End component can be written to understand URL Signing.
For example, a User which does not have DELETE access to a data item
may have the delete button Grayed out and disabled.