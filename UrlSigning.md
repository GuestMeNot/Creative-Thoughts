## URL Signing

[Signed URLs](https://cloud.google.com/cdn/docs/using-signed-urls) 
provide limited access to certain URLS.

Signed URLs can be sent to a caller so that a caller knows which
[Rest](https://en.wikipedia.org/wiki/Representational_state_transfer) URLs they can call.
Suppose a Rest Service has the following endpoints:
 
- Get a List of Data via a [Post]() 
- Get a single data item via a [Get]()
- Update a single data ite via a [Post]()
- Update

For any given set of URLs can be signed.