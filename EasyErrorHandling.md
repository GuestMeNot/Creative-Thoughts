## Easy Error handling

Server logs are often sprinkled with clutter. In a stateless cloud environment, debugging
a problem often requires collating logs across multiple servers.

Several touch points are required to quickly resolve issues:
- Operations staff want to know which errors are actionable on their part and where the root cause of the
problem is. 
- Users want expedited fixes to their specific problem. If the problem is on the server they
expect that a solution will be found quickly. If the issue is data entry, they expect the system to tell them 
as soon as possible how to resolve the error.
- Cyber-Security does not want errors to leak sensitive information to the caller. 
- Database specific issues should be automatically directed to one set of operations staff, while 
- Network specific issues should be directed to another set of operations staff. 
- Automated log monitoring tools should see this data in a consistent way across a suite of applications to reduce rewriting similar logic over 
and over again.

Exceptions can be categorized into exception types based on divergent needs and sent to the appropriate
folks. This could be handled in a simple component which is called by when errors are encountered.

Each Error can be logged with a Unique Identifier such as a [UUID](https://en.wikipedia.org/wiki/Universally_unique_identifier) 
which is also sent to the user. this helps to locate the exact Error in the logs.

Easy Error Handling can convert exception types into error categories. For example:
Database Specific Exceptions and Network Exceptions can each be logged with a Unique Identifier
such as "Database Error" or "Network Error". These groupings could be unique to each situation
or generalized to accommodate a suite of applications. Log monitoring Systems can
the read these values and direct the errors to the appropriate location.




