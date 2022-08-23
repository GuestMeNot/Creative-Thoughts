### Thoughts on a Distributed Mutex

Often when creating concurrent data structures a mutex is used to guard access
to the underlying data. What if we had a Distributed Mutex? Then many of
the concurrent data structures could become distributed by copying the mutex
and the associated data. The Distributed Mutex would not presuppose a specific
implementation for a Distributed HashMap or a Distributed Array as these
choices would be dependent on additional criteria. 

A naive HashMap implementation might just drop in a Distributed Mutex for an
existing Mutex, but this is probably not the best approach.

### Design

1. An attempt to acquire the Mutex guard is made.
2. The Mutex checks if it currently has an internal lock locally or whether it must request the lock.
3. If the Mutex has the internal lock locally and no other code is requesting the Mutex then the lock is granted.
    - If the internal lock is granted and the data associated with the lock is modified, the data is
      copied to another mutex holder lazily when they request access to the lock.
4. If the mutex does not have the internal lock locally a request is made to the lock holder to gain access to the lock.
5. If the Mutex lock holder does not have any requests for the internal lock then:
    - It sends the Mutex internal lock to the lock requester along with any updated mutex data
    - It informs other participants in the Mutex that the internal lock has moved to a new lock holder.
6. If the internal lock changes locations while a lock requester is asking for 
   it then the requester asks for the internal lock from the new lock holder.
7. If a lock requester cannot reach a lock holder then the remaining members
    can vote to allow the new requester to access the lock using a consensus 
    mechanism.
    - When the previously unreachable prior lock holder comes online, they are informed
        that they lost the lock and the prior lock holder will roll back the changes
        to a prior state.

### Gotchas

There are a lot of issues to be worked through:

- What does the consensus mechanism look like?
- Which protocol is used?
- How does one participate in a mutex?
- How does the code roll back the changes for a mutex?
- It is assumed that a Mutex looks like a Rust Mutex what if it isn't?
- There are several other concerns discussed [here](https://cliffle.com/blog/rust-mutexes/).
- More research needs to be done on Distributed Mutexes prior to implementation
    as other designs may be better or may inform this design.
- There are could subtle usage scenarios, which need to be thought through.
- How is the internal lock represented such that all participants can self-recognize 
  who has the internal lock?
- How would multiple Distributed Mutexes in an application share network connections?
- What happens to downstream code when the Mutex is rolled back?
- What happens if the network is partition evenly across participants?
