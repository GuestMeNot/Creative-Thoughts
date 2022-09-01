### Decentralized Digital Certificates

An issue with creating Digital Certificates without a centralized 
authority is validating identity of an Actor.

Assuming the physical street address and legal name of the owner of
a digital certificate are not needed as part of the certification process
(as is often the case in Blockchain applications) this approach can be used.

This example uses Blockchain as an immutable reference mechanism, but other
approaches are possible.

### Why do we need digital certificates
Over and above Confidentiality, Access-Control and Data Integrity,
available through other cryptographic means, we would like the following:

Identification / Authentication:
- We want like to be able to trace down other problems in the system.
- Not needed for Legal reasons so the Legal name and address of the certificate holder are not required. 

Non-Repudiation:
- We want to identify the creator of a piece of data.
- If the data creates trouble we would like to identify the responsible party. 
  For example, suppose someone is always creating bad transactions. We would like
  to know the reason, so we can increase the total throughput in the system.
  Is the reason due to incompatibility, bad programming, or something else.

### Blockchain based digital certificate authority.

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

### LOTOS specification

This section outlines the certificate creation process in 
[LOTOS](https://en.wikipedia.org/wiki/Language_Of_Temporal_Ordering_Specification).

[WIP - not valid lotos]

    (
      (no_peers; only_genesis_block_in_chain; create_certificate; sign_certificate; create_genesis_certificate_block; write_certificate_to_storage; await_peers)
      ||
      (peer_available; no_certificate_in_storage; not_genesis_block_creator; await_peers; create_certificate; send_certificate_to_peer; receive_certificate_block_number_from_peer; write_certificate_to_storage; await_peers))
      ||
      (peer_available; no_certificate_in_storage; not_genesis_block_creator; await_peers; create_certificate; send_certificate_to_peer; receive_certificate_invalid_from_peer; exit))
      ||
      (no_peers; certificate_in_storage; await_peers; send_certificate_to_peer; receive_certificate_block_number_from_peer; await_peers)
      ||
      (no_peers; certificate_in_storage; await_peers; send_certificate_to_peer; receive_certificate_invalid_from_peer; exit)
      || 
      (peer_available; send_certificate_to_peer; receive_certificate_block_number_from_peer; await_peers)
      ||
      (peer_available; send_certificate_to_peer; receive_certificate_invalid_from_peer; exit)
    )
    |await_peers|
    (
      (await_peers; receive_certificate_from_peer; certificate_not_already_registered; sign_certificate; create_certificate_block; send_certificate_block_number_to_peer; await_peers)
      ||
      (await_peers; receive_certificate_from_peer; certificate_already_registered; verify_certificate_signature; send_certificate_block_number_to_peer; await_peers)
      ||
      (await_peers; (recieve_invalid_certificate_from_peer; tell_peer_certificate_was_invalid) || (no_peer_response; exit))
      ||
      (await_peers; recieve_invalid_certificate_from_peer; tell_peer_certificate_was_invalid)
    )

### CID

One alternative to this system might be to use 
[IPFS CIDs](https://docs.ipfs.tech/concepts/content-addressing/) 
to store certificates.

### Further Investigation

- A formal specification 
  [Process Calculi](https://en.wikipedia.org/wiki/Language_Of_Temporal_Ordering_Specification)
  could also be used.
