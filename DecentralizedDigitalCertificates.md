### Decentralized Digital Certificates

An issue with creating Digital Certificates without a centralized 
authority is validating the identity of a requester.

Assuming the physical street address and legal name of the owner of
a digital certificate are not needed as part of the certification process 
as is often the case in Blockchain applications this approach can be used.

### Blockchain based digital authority.

In blockchain applications trust is implicitly given to the original
creator of the genesis block. This trust can be leveraged to create
a trusted certificates. Along with a genesis block this original 
server (or genesis block creator) can create and add a self-signed digital 
certificate to the blockchain. 

When a new peer joins with the original genesis block creator it can
send over a self-signed certificate. This new certificate can then
be written to the blockchain and signed by the genesis block creator.

When another new peer joins, it sends over a self-signed digital
certificate to either of the original two peers. The receiving peer
would then sign the digital certificate and add it to the blockchain.

To boostrap the system X number of peers with associated digital 
certificates can be created without GAS. Afterwards, GAS will need 
to be paid to prevent misuse.

### Implications

One interesting implication of this approach is that after X peers have joined
any addition peer who wishes to join must already have sufficient GAS.
This could be seen as positive since the distributed system will not be 
flooded with random peers requesting to join. It could also be seen as
negative because it limits the potential pool of additional peers.

### CID

One alternative to this system might be to use 
[IPFS CIDs](https://docs.ipfs.tech/concepts/content-addressing/) 
to store the certificates.

### Further Investigation

- A formal specification such as
  [LOTOS](https://en.wikipedia.org/wiki/Language_Of_Temporal_Ordering_Specification),
  [Process Calculi](https://en.wikipedia.org/wiki/Language_Of_Temporal_Ordering_Specification)
  etc. should be used.