---
NEP: 999
Title: Non-transferable NFT Standard
Author: Jasper Timm <jasper@kycdao.xyz>, Sándor Juhász <sandor@kycdao.xyz>, Balazs Nemethi <balazs@kycdao.xyz>
DiscussionsTo: https://github.com/nearprotocol/neps/pull/0999
Status: Draft
Type: Standards Track
Category: Contract
Created: 02-May-2022
Requires: 171
---

## Summary

A standardised interface for constructing NFTs (non fungible tokens) that are not meant to be transfered. These non-transferable NFTs (NTNFTs, or account bound NFTs) should be bound to an owner address on minting so the owner never changes.

## Motivation

There are cases when a service would like to attribute something to a specific address and only that address. In such cases these attestations would no longer be applicable if they were transfered to a different account.

Example use-cases:

- badges, POAPs
- proofs of KYC or other verifications
- ticketing, access control
- certificates

As each of these attestations would be unique, it also makes sense to build upon the existing infrastructure already built for NFTs by basing this new standard on [NEP-171](). This would mean existing software, such as wallets, have 'backwards compatibility' with these tokens, allowing it to display simple details such as title, symbol and images.

In addition, having the non-transferable nature standardised would mean that in the future, software could give users a hint in the UI that these tokens are account bound and disabling any transfer functionality.

## Rationale and alternatives

One possible alternative to this standard would be to implement the non-transferable nature by creating a NEP-171 contract whose transfer function is prohibited (or extremely limited). However, recognising this fact would be hard to determine without checking the code of the contract. Making this limitation explicit, via a standard, would build trust in the owners of tokens abiding to this new NEP standard.

## Specification

[TODO - Needs more explanation]()From a technical standpoint the functionalities described by this standard will be a subset of the standard NFT functions, so this is not really an “extension” of the NEP-171, it is actually a subset.

## Reference Implementation

The interface of an NTNFT contract is really simple, it would “inherit” the only function from NEP-171 which is not related to transferring tokens:

`function nft_token(token_id: string): Token|null {}`

Although not explicitly required by this standard, the existing Metadata and Enumeration standards may extend the interface by the usual functions, for example: `nft_metadata, nft_total_supply, nft_supply_for_owner`

[TODO]():
- More in-depth description with source code for the implementation.
- Discuss examples described above and how they'd use these functions.

## Unresolved Issues

### The ability to transfer NTNFTs due to lost/stolen or new account

Worth considering is the case of what should happen to these NTNFTs if the user loses access to their account, it is somehow compromised or they simply wish to use a new account. As there are a number of ways this can be handled, this standard does not explicitly mandate that any particular function MUST be implemented to handle this.

However, the following method MAY be considered:
- The service which created the smart contract would make efforts (off-chain) to identify the user. Once they're satisfied, at the user's request, they would then revoke the NTNFT in the user's old account and mint a new token (presumably with the same metadata) to the new account. This would require adding: `function burn_token(token_id: string): null {}`, which would be callable only by the contract creator.
- [TODO]() - Are there other common ways to handle this?

### Preventing transfer of account via trading wallet keys

There will always exist the possibility that a user can simply transfer all the assets in their account, including any NTNFTs, to another user by simply giving them the private keys to the account.

Ultimately, there is no way to prevent this. However, given common methods such as the one listed above for a user to do some sort of authenticated account recovery process, it would mean that any 'buyer' would run the risk the 'seller' simply recovers all their tokens using this process making such a trade less likely.

## Future possibilities

There are a few natural extensions to this standard which might be considered in NEPs for the future.

The first is setting a standard for an 'authenticated account recovery' process. If such a process, like that described above, were standardised it would give users confidence that their NTNFTs would not be lost if they need to migrate to a new account. It would also enable frontend software to assist users in the account recovery process when they know the contract supports it.

Other future NEPs might focus on more specific metadata which the NTNFT would provide, for specific use-cases. An example could be for when the NTNFT is used for KYC purposes that the metadata should specifically include fields such as `expiry date, validity and KYC_level`.

## Copyright
[copyright]: #copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
