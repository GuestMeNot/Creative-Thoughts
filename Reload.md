### Web Data Reload

Imagine you have been working on a web application, and for some reason
you decide to exit it. Later you realized you need to change something 
that you did earlier. You log back into the application only to discover
that you need to get back to where you were, but you don't remember 
the exact details.

We can solve this problem by storing our data directly in the browser
using [Web Storage](https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API).
In many situations, there are additional security concerns which prevent this
as a solution. In other situations the data may have changed on the server itself
so that the old data is stale.

We can instead store our Rest calls in Web Storage and reload data from the server
when the user logs in. In a [single page webapp](https://www.monocubed.com/blog/what-is-single-page-application/), 
we can additionally store the latest set of navigation data in Web Storage. 