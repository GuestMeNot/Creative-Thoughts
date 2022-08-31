### Decentralized Digital Certificates

An issue with creating Digital Certificates without a centralized 
authority is validating the identity of a requester.

Assuming the physical street address and official name are not
needed as part of the certification process as is often the case 
in Blockchain applications this approach can be used.

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

### CID

One alternative to this system might be to use 
[IPFS CIDs](https://docs.ipfs.tech/concepts/content-addressing/) 
to store the certificates.