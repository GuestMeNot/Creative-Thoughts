
Here are a few random thoughts.

Toward [DRYer](https://medium.com/kite-srm/what-writing-dry-code-really-means-a8bb031289c9) 
code from an architectural perspective:

- [Easy Error Handling](EasyErrorHandling.md) ensures multiple stakeholders receive the information
  the need in a timely manner without additional noise.

- [Simplified Field Validation](Validation.md) keeps field validation code in sync between 
  Browser code and server code.

- [Unmodifiable data and Invisible Data](UnmodifiableAndInvisibleData.md) can be used to 
  either hide data from a user while making updates simpler. For example, a person has an 
  [SSN](https://www.ssa.gov/ssnumber/) which for security reasons should only be visible to a 
  limited set of users. To update a person's data, the SSN should be included in the update 
  but should be invisible to users who do not have the proper permissions. Likewise, there are 
  pieces of data which should be visible to a user such as Latitude and Longitude of a Geolocation, 
  but which should never be updated. Writing custom code for each of these situations is redundant.
  This should be checked on the server but also handled on the front-end so that components
  can automatically detect what is possible with a particular piece of data.

- [URL Signing](UrlSigning.md) guarantees that only authorized users can update data fields 
  and enables disables or hides buttons for which the user does not have Authorization.

Improving User Experiences:

- [Opinionated Data Field Lifecycle States](OpinionatedDataFieldLifecycle.md) visually informs uses of
  the state the data is in at a glance.

- [Reloading Data State of an Application](Reload.md) brings the user back to
  the state they were in when the application last exited.

