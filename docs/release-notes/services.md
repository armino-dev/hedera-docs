---
description: Hedera Services release information
---

# Hedera Services

For the latest versions supported on each network please visit the Hedera status [page](https://status.hedera.com).

## Upcoming Releases

## [v0.24](https://github.com/hashgraph/hedera-services/releases/tag/v0.24.0)

{% hint style="info" %}
**MAINNET UPDATE: APRIL 15, 2022**
{% endhint %}

{% hint style="success" %}
**TESTNET UPDATE:  MARCH 31, 2022**
{% endhint %}

{% hint style="success" %}
**TESTNET UPDATE:  MARCH 24, 2022**
{% endhint %}

In the 0.24 release of Hedera Services, we are excited to give smart contract developers a new level of interoperability with native Hedera Token Service (HTS) tokens via [HIP-218 (Smart Contract interactions with Hedera Token Accounts)](https://hips.hedera.com/hip/hip-218). The Hedera EVM now exposes every HTS fungible token as an ERC-20 token at the address of the token’s `0.0.X` entity id; and analogously, every HTS non-fungible token appears as an ERC-721 token. This means a smart contract can look up its balance of a fungible HTS token; or change its behavior based on the owner of a particular HTS NFT. Please see the linked HIP for full details.

This upgrade also creates two new system accounts 0.0.800 and 0.0.801 that will hold reward funds.

One change to the Hedera API (HAPI) is that we now have enough evidence to conclude the experimental `getAccountNftInfos` and `getTokenNftInfos` queries do not have a favorable cost/benefit ratio, and these queries are now [permanently disabled](https://hashgraph.github.io/hedera-protobufs/#proto.TokenService).



## Latest Releases &#x20;

## [v0.23](https://github.com/hashgraph/hedera-services/releases/tag/v0.23.0)

{% hint style="success" %}
**MAINNET UPDATE: MARCH 10, 2022**
{% endhint %}

{% hint style="success" %}
**TESTNET UPDATE:  FEBRUARY 17, 2022**
{% endhint %}

Hedera Services 0.23 fleshes out our smart contract service via the implementation of [HIP-329 (Support `CREATE2` opcode)](https://hips.hedera.com/hip/hip-329). Smart contract developers are now free to use the `CREATE2` EVM opcode. A typical use case is a distributed exchange that wants its pair contracts to have deterministic addresses based on the tokens in the pair.&#x20;

Please note two issues fixed in this release. [First](https://github.com/hashgraph/hedera-services/issues/2841), in release 0.22, the nodes returned the `bytes ledger_id` stipulated by [HIP-33](https://hips.hedera.com/hip/hip-33) as a UTF-8 encoding of a hex string. The returned bytes are now the big-endian representation of the ledger's numeric id. [Second](https://github.com/hashgraph/hedera-services/issues/2857), prior to this release, the record of a `dissociateToken` from a deleted token did not list the discarded balance of the dissociated account if the token's treasury was missing. This is now fixed.

![](<../../.gitbook/assets/Performance Measurement Results\_Extract.001 (2).jpeg>)

## [v0.22](https://github.com/hashgraph/hedera-services/releases/tag/v0.22.1)

{% hint style="success" %}
**MAINNET UPDATE: FEBRUARY 3, 2022**
{% endhint %}

{% hint style="success" %}
**TESTNET UPDATE:  JANUARY 20, 2022**
{% endhint %}

The 0.22 release is a paradigm shift for Hedera Services, as we deliver the next major step in our Smart Contracts 2.0 roadmap on the strength of the protean [HIP-25](https://hips.hedera.com/hip/hip-25), a technical foundation for scaling the world state of our ledger to billions of entities _without_ sacrificing the high TPS enabled by the hashgraph consensus algorithm.

Highlights of this release include:

* Network EVM capacity increased to 15M `gas`-per-second. (Please see [HIP-185](https://hips.hedera.com/hip/hip-185) for details.)
* Gas limit per `ContractCreate` or `ContractCall` raised to 4M.
* Per-contract storage capacity increased to 10MB.
* Solidity integration with native HTS tokens. (Please see [HIP-206](https://hips.hedera.com/hip/hip-206) for details.)

We expect more progress in these directions over the coming releases. Do note that the gas usage of the HTS integrations is still evolving; follow [this issue](https://github.com/hashgraph/hedera-services/issues/2786) to track the finalized gas charges leading up to mainnet release.

There are two other HIP's included in this release not related to the smart contract service. First, [HIP-33](https://hips.hedera.com/hip/hip-33) enhances queries like `CryptoGetInfo` with a _ledger id_ that marks which Hedera network answered the query. Second, [HIP-31](https://hips.hedera.com/hip/hip-31) allows a client to include the expected decimals for a token in a `CryptoTransfer`. This means a hardware wallet can guarantee its token transactions will have the precision seen by the the user in the device display.

While we are gaining momentum in our smart contracts roadmap, we are also deeply committed to improving the developer experience, and welcome issues and ideas in our [GitHub repository](https://github.com/hashgraph/hedera-services) and [Discord](https://hedera.com/discord)!

![](<../../.gitbook/assets/Performance Measurement Results\_Extract.001 (1).jpeg>)

## [v0.21.0](https://github.com/hashgraph/hedera-services/releases/tag/v0.21.0-rc.1)

{% hint style="success" %}
**MAINNET UPDATE: JANUARY 13, 2022**
{% endhint %}

{% hint style="success" %}
**TESTNET UPDATE:  DECEMBER 21, 2021**
{% endhint %}

In Hedera Services 0.21 we are pleased to announce support for [ECDSA(secp256k1) keys](https://hips.hedera.com/hip/hip-222) and [auto-account creation](https://hips.hedera.com/hip/hip-32).

The Ethereum network makes heavy use of ECDSA cryptography with the secp256k1 curve, and by supporting these keys we ease the developer experience of migrating a dApp to Hedera. Anywhere a Ed25519 key can be used in the Hedera API, it is now possible to substitute an ECDSA(secp256k1) key.

Auto-account creation lets a new user receive ℏ via a `CryptoTransfer` _without_ having already created an `0.0.X` id on the network. The new user only needs to provide their public key, and when a sponsor account sends ℏ "to" their key via a new [`AccountID.alias` field](https://hashgraph.github.io/hedera-protobufs/#proto.AccountID), the network automatically creates an account with their key. Additional transfers to and from an auto-created account may also use its alias instead of the account id.&#x20;

An alias may also be used to get the account balance and account info for the account. (Do note there is a [known issue](https://github.com/hashgraph/hedera-services/issues/2653) that causes the `getAccountInfo` query response to echo back the account alias instead of its `0.0.<num>` id; this will be fixed in the next release. Please use the free `getAccountBalance` query to check the `0.0.<num>` id that corresponds to an alias.) You will be able to use the alias in all other transactions and queries in a future release.

Meanwhile, our team continues exhaustive due diligence for Smart Contracts 2.0... 🚀

![](<../../.gitbook/assets/Performance Measurements.jpeg>)

## [v0.20.0](https://github.com/hashgraph/hedera-services/releases/tag/v0.20.0)

{% hint style="success" %}
**MAINNET UPDATE: DECEMBER 2,2021**
{% endhint %}

{% hint style="success" %}
**TESTNET UPDATE: NOVEMBER 18, 2021**
{% endhint %}

Hedera Services 0.20 is primarily a scaffolding release, as our team is working heads-down to deliver the Smart Contract Service refresh with massive new scale and performance; as well as smart contract integration with native tokens created using the Hedera Token Service. The scope of this refresh is significant, and we believe it will be well worth the wait.

The main deliverables in this release are improved automation for node operators to use in software upgrades; and a handful of minor bug fixes, including for <mark style="color:purple;"></mark> [<mark style="color:purple;">#2432</mark>](https://github.com/hashgraph/hedera-services/issues/2432).

Please also note the following deprecations in the Hedera API protobufs:

* The [<mark style="color:purple;">`ContractUpdateTransactionBody.fileID`</mark> <mark style="color:purple;"></mark><mark style="color:purple;">field</mark>](https://github.com/hashgraph/hedera-protobufs/blob/main/services/contract\_update.proto#L82), which is redundant given the existence of the <mark style="color:purple;"></mark> [<mark style="color:purple;">`ContractGetBytecode`</mark> <mark style="color:purple;"></mark><mark style="color:purple;">quer</mark>y](https://github.com/hashgraph/hedera-protobufs/blob/main/services/smart\_contract\_service.proto#L63).
* The [<mark style="color:purple;">`ContractCallLocalQuery.maxResultSize`</mark> <mark style="color:purple;"></mark><mark style="color:purple;">field</mark>](https://github.com/hashgraph/hedera-protobufs/blob/main/services/contract\_call\_local.proto#L136), as this limit is now simply a side-effect of the given gas limit.

![](<../../.gitbook/assets/Performance Measurement Results\_Extract.001 (1) (1) (1).jpeg>)

## [v0.19.4](https://github.com/hashgraph/hedera-services/releases/tag/v0.19.4)

{% hint style="success" %}
**MAINNET UPDATE: NOVEMBER 4,2021**
{% endhint %}

{% hint style="success" %}
**TESTNET UPDATE: OCTOBER 28, 2021**
{% endhint %}

## ****[v0.19.3](https://github.com/hashgraph/hedera-services/releases/tag/v0.19.1)

{% hint style="success" %}
**TESTNET UPDATE:  OCTOBER 21, 2021**
{% endhint %}

In Hedera Services 0.19, we are thrilled to announce migration of the Hedera smart contract service to the Hyperledger Besu EVM, as laid out in [HIP-26](https://github.com/hashgraph/hedera-improvement-proposal/blob/master/HIP/hip-26.md). This enables support for the latest v0.8.9 Solidity contracts, and harmonizes our gas schedule with that of the “London” hard fork. The Besu migration also sets the stage for a step change in smart contract performance on Hedera.

Two other HIPs targeting the Hedera Token Service go live in this release. First, the [HIP-23](https://github.com/hashgraph/hedera-improvement-proposal/blob/master/HIP/hip-23.md) feature set is now enabled, so that any account that has been configured with a non-zero `maxAutoAssociations` can receive air-drops (i.e., units or NFTs of a token type without explicit association). Second, we have also implemented [HIP-24](https://github.com/hashgraph/hedera-improvement-proposal/blob/master/HIP/hip-24.md), which provides a safety measure for token types created with a `pauseKey`. If a `TokenPause` is submitted with this key’s signature, then all operations on the token will be suspended until a subsequent `TokenUnpause`.

## [v0.18.1](https://github.com/hashgraph/hedera-services/releases/tag/v0.18.1)

{% hint style="success" %}
**MAINNET UPDATE: OCTOBER 7, 2021**
{% endhint %}

In Hedera Services 0.18.1, we have a new scalability profile for NFTs in the Hedera Token Service (HTS). Up to fifty million (50M) NFTs, each with 100 bytes of metadata, may now be minted. Of course our `CryptoTransfer` and `ConsensusSubmitMessage` operations are still supported at 10k TPS even with this scale.

In this release, we have also enabled automatic reconnect. This feature comes into play when a network partition causes a node to "fall behind" in the consensus protocol. With reconnect enabled, the node can use a special form of gossip to "catch up" and resume participation in the network with no human intervention. This works even when the node has missed many millions of transactions, and the world state is very different from when it was last active.

We are happy to also announce that accounts can be customized to take advantage of the upcoming [HIP-23 (Opt-in Token Associations)](https://github.com/hashgraph/hedera-improvement-proposal/blob/master/HIP/hip-23.md) feature set. That is, an account owner can now "pre-pay" for token associations via a [`CryptoCreate`](https://hashgraph.github.io/hedera-protobufs/#proto.CryptoCreateTransactionBody) or [`CryptoUpdate`](https://hashgraph.github.io/hedera-protobufs/#proto.CryptoUpdateTransactionBody) transaction, _without_ knowing in advance which specific token types they will use.

Once HIP-23 is fully enabled in release 0.19, when their account receives units or NFT's of a new token type via a `CryptoTransfer`, the network will automatically create the needed association---no explicit `TokenAssociate` transaction needed. This supports several interesting use cases; please see the linked HIP-23 for more details.

There are three other points of interest in this release.

First, we have removed the HIP-18 limitations noted in the previous release. The `tokenFeeScheduleUpdate` transaction has been re-enabled, and multiple royalty fees can now be charged for a non-fungible token type.

Second, the address books in system files `0.0.101` and `0.0.102` will now populate their `ServiceEndpoint` fields. (However, the deprecated `ipAddress`, `portno`, and `memo` fields will no longer be populated after the next release.)

Third, please note that the `TokenService` `getTokenNftInfos` and `getAccountNftInfos` queries are now **deprecated** and will be removed in a future release. The best answers to such queries demand historical context that only Mirror Nodes have; so these and related queries will move to mirror REST APIs.

Developers will likely appreciate two other release 0.18.1 items. First, we have migrated to [Dagger2](https://dagger.dev) for dependency injection. Second, there is a new `getExecutionTime` query in the [`NetworkService`](https://hashgraph.github.io/hedera-protobufs/#proto.NetworkService) that supports granular performance testing in development environments.

![](../../.gitbook/assets/image.png)

## v0.18.0

{% hint style="success" %}
**TESTNET UPDATE:  SEPTEMBER 23, 2021**
{% endhint %}

In Hedera Services 0.18.0, we are happy to announce support for [HIP-23 (Opt-in Token Associations)](https://github.com/hashgraph/hedera-improvement-proposal/blob/master/HIP/hip-23.md). This feature lets an Hedera account owner "pre-pay" for token associations via a [`CryptoCreate`](https://hashgraph.github.io/hedera-protobufs/#proto.CryptoCreateTransactionBody) or [`CryptoUpdate`](https://hashgraph.github.io/hedera-protobufs/#proto.CryptoUpdateTransactionBody) transaction, _without_ knowing in advance which specific token types they will use.

Then, when their account receives units or NFT's of a new token type via a `CryptoTransfer`, the network automatically creates the needed association---no explicit `TokenAssociate` transaction needed. This supports several interesting use cases; please see the linked HIP-23 for more details.

There are three other points of interest in this release.

First, we have removed the HIP-18 limitations noted in the previous release. The `tokenFeeScheduleUpdate` transaction has been re-enabled, and multiple royalty fees can now be charged for a non-fungible token type.

Second, the address books in system files `0.0.101` and `0.0.102` will now populate their `ServiceEndpoint` fields. (However, the deprecated `ipAddress`, `portno`, and `memo` fields will not be no longer be populated after the next release.)

Third, please note that the `TokenService` `getTokenNftInfos` and `getAccountNftInfos` queries are now **deprecated** and will be removed in a future release. The best answers to such queries demand historical context that only Mirror Nodes have; so these and related queries will move to mirror REST APIs.

Developers will likely appreciate two other release 0.18.0 items. First, we have migrated to [Dagger2](https://dagger.dev) for dependency injection. Second, there is a new `getExecutionTime` query in the [`NetworkService`](https://hashgraph.github.io/hedera-protobufs/#proto.NetworkService) that supports granular performance testing in development environments.

**Performance Measurement Results:**

![](../../.gitbook/assets/performance-measurement-results.jpeg)

## [v0.17.4](https://github.com/hashgraph/hedera-services/releases/tag/v0.17.3)

{% hint style="success" %}
**MAINNET UPDATE: SEPTEMBER 2, 2021**
{% endhint %}

{% hint style="success" %}
**TESTNET UPDATE:  AUGUST 30, 2021**
{% endhint %}

In Hedera Services 0.17.2, we are excited to announce support for  [HIP-17 (Non-fungible Tokens)](https://github.com/hashgraph/hedera-improvement-proposal/blob/master/HIP/hip-17.md), with a complementary extension to [HIP-18 (Custom Hedera Token Service Fees)](https://github.com/hashgraph/hedera-improvement-proposal/blob/master/HIP/hip-18.md) that lets an NFT creator set a royalty fee to be charged when fungible value is exchanged for one of their creations.

Unique token types and minted NFTs are more natural for many use cases than fungible token types. The Hedera Token Service now supports both natively, so that a single `CryptoTransfer` can perform atomic swaps with any arbitrary combination of fungible, non-fungible, and ℏ transfers. (Please do note that the "paged" `getAccountNftInfos` and `getTokenNftInfos` queries will remain disabled until release 0.18.0, as several large performance improvements are pending.)

In this release we have made it possible to denominate a fixed fee in the units of the token to which it is attached (assuming the type of this token is `FUNGIBLE_COMMON`). Custom fractional fees may now also be set as "net-of-transfer". In this case the recipient(s) in the transfer list receive the stated amounts, and the assessed fee is charged to the sender.

There are a few final points of more specialized interest. First, users of the scheduled transaction facility may now also schedule `TokenBurn` and `TokenMint` transactions. Second, network administrators issuing a `CryptoUpdate` to change the treasury account's key must now sign with the new treasury key. Third, the supported TLS cipher suites have been updated to the following list:

1. `TLS_DHE_RSA_WITH_AES_256_GCM_SHA384` (TLS v1.2)
2. `TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384` (TLS v1.2)
3. `TLS_AES_256_GCM_SHA384` (TLS v1.3)

⚠️ There are two temporary limitations to HIP-18 in this release. First, the `tokenFeeScheduleUpdate` transaction is not currently available. Second, only one royalty fee will be charged for a non-fungible token type. Both limitations will be removed in release 0.18.0.

#### Performance Measurement Results:

![](../../.gitbook/assets/0.17.png)



## [v0.17.3](https://github.com/hashgraph/hedera-services/releases/tag/v0.17.3-rc.1)

{% hint style="success" %}
**TESTNET UPDATE:  AUGUST 24, 2021**
{% endhint %}

Please see 0.17.4 release notes.

## [v0.17.2](https://github.com/hashgraph/hedera-services/releases/tag/v0.17.2)

{% hint style="success" %}
**TESTNET UPDATE:  AUGUST 19, 2021**
{% endhint %}

Please see 0.17.4 release notes.

## [v0.16.1](https://github.com/hashgraph/hedera-services/releases/tag/v0.16.1)

{% hint style="success" %}
**MAINNET UPDATE COMPLETED: AUGUST 5, 2021**
{% endhint %}

{% hint style="success" %}
**TESTNET UPDATE COMPLETED:  JULY 22, 2021**
{% endhint %}

In Hedera Services 0.16.0, we are excited to announce support for [HIP-18 (Custom Hedera Token Service Fees)](https://github.com/hashgraph/hedera-improvement-proposal/blob/master/HIP/hip-18.md).

Hedera tokens can now be created with a schedule of up to 10 custom fees, which are either _fixed_ in units of ℏ or another token; or _fractional_ and computed in the units of the owning token. The ledger automatically charges custom fees to accounts as they send units of a fungible token (or ownership of a NFT, see below) via a `CryptoTransfer`.

When a custom fee cannot be charged, the `CryptoTransfer` fails atomically, changing no balances other than for the Hedera network fees.

The five case studies in [this document](https://github.com/hashgraph/hedera-services/blob/master/docs/fees/custom-fees-characterization.md) show the basics of how custom fees are charged, and how they appear in records. Note that at most two "levels" of custom HTS fees are allowed, and custom fee-charging cannot require changing more than 20 account balances.

⚠️  There is one variation on custom fees that requires a work-around in this release. Specifically, if a fixed fee should be collected _in the units of the "parent" token to whose schedule it belongs_, then in Release 0.16.0 this must be accomplished using a `FractionalFee` as described in [this issue](https://github.com/hashgraph/hedera-services/issues/1925). In Release 0.17.0 the more natural `FixedFee` configuration will be available.

In this release, we have also enabled previewnet support for [HIP-17 (Non-fungible Tokens)](https://github.com/hashgraph/hedera-improvement-proposal/blob/master/HIP/hip-17.md). Unique token types and minted NFTs are more natural for many use cases than fungible token types. The Hedera Token Service will soon support both natively, so that a single `CryptoTransfer` can perform atomic swaps with any arbitrary combination of fungible, non-fungible, and ℏ transfers.

We are very grateful to the Hedera user community for these interesting and powerful new feature sets.

#### Performance Measurement Results:

![](../../.gitbook/assets/0.16.1.png)

## [v0.15.1](https://github.com/hashgraph/hedera-services/releases/tag/v0.15.1)

{% hint style="success" %}
**MAINNET UPDATE COMPLETED: JULY 1, 2021**
{% endhint %}

{% hint style="success" %}
**TESTNET UPDATE COMPLETED: JUNE 17, 2021**&#x20;
{% endhint %}

In Hedera Services 0.15.1, we improved performance and integrated with the latest Platform SDK to enable full support of network reconnect.

These performance improvements let us augment the Hedera world state with records of all transactions handled in the three minutes of consensus time, even when handling 10,000 transactions per second. The HAPI `GetAccountRecords` query now returns, from state, all such records for which the queried account was the payer account.

We have also finalized the design for the non-fungible token (NFT) support to be added to the Hedera Token Service (HTS) in release 0.16.0. The protobufs for new HAPI operations are available in the 0.15.0 tag of the[ hedera-protobufs](https://github.com/hashgraph/hedera-protobufs) GitHub repository.\
\
To simplify fee calculations, there is now a maximum entity lifetime of a century for any entity whose lifetime is not \_already\_ constrained by the maximum auto-renew period. A HAPI transaction that tries to set an expiration further than a century from the current consensus time will resolve to `INVALID_EXPIRATION_TIME`.

## [v0.14.0](https://github.com/hashgraph/hedera-services/releases/tag/0.14.0)

{% hint style="success" %}
**MAINNET UPDATE COMPLETED: JUNE 3, 2021**
{% endhint %}

{% hint style="success" %}
**TESTNET UPDATE COMPLETED: MAY 20, 2021**&#x20;
{% endhint %}

In Hedera Services 0.14.0, we have implemented account auto-renewal according to the specifications of [HIP-16](https://github.com/hashgraph/hedera-improvement-proposal). This feature will not be enabled until a later date, after ensuring universal awareness of its impact in the user community.

This release includes notable infrastructure work to enable use of the Platform reconnect feature. Reconnect allows a node that has fallen behind in consensus gossip to catch back up dynamically.&#x20;

A minor improvement to the Hedera API is that the GetVersionInfo query now includes the optional pre-release version and build metadata fields from the Semantic Versioning spec (if applicable).

To simplify life for system admins who are updating a system account's key, we now waive the signing requirement for the account's new key.

## [v0.13.2](https://github.com/hashgraph/hedera-services/releases/tag/v0.13.2)

{% hint style="success" %}
**MAINNET UPDATE COMPLETED: MAY 6, 2021**
{% endhint %}

{% hint style="success" %}
**TESTNET UPDATE COMPLETED: APRIL 29, 2021 \[v0.13.2]**
{% endhint %}

{% hint style="success" %}
**TESTNET UPDATE COMPLETED: APRIL 22, 2021 \[v0.13.0]**
{% endhint %}

In Hedera Services v0.13.0, we have [redesigned](https://github.com/hashgraph/hedera-services/blob/master/docs/scheduled-transactions/revised-spec.md) schedule transactions. The new design gives collaborating nodes a well-defined workflow if they happen to schedule identical transactions, _even if_ they are using different gRPC client libraries (for example, Go and JavaScript). The new design also reduces the number of signatures required to submit a valid `ScheduleSign` transaction in many common use cases. Users will be able to schedule `CryptoTransfer` and `ConsensusSubmitMessage` transactions in this release. Other transaction types will be introduced in future releases.

{% hint style="warning" %}
**Note:** The schedule transactions feature will not be enabled in this release; it's expected to be enabled on testnet in a subsequent v0.13.2 update on April 29th. This feature is enabled on previewnet.
{% endhint %}

This release deprecates three fields in the [protobuf](https://hashgraph.github.io/hedera-protobufs/#proto.NodeAddress) for system files `0.0.101` and `0.0.102`. The three deprecated fields are `ipAddress`, `portno`, and `memo`. When we rely on these fields, we cannot concisely represent node with multiple IP addresses. For example, take mainnet node 0 (account `0.0.3`), which as of this writing has proxy IPs `13.82.40.153`, `34.239.82.6`, and `35.237.200.180`. The mainnet `0.0.101` file must include a `NodeAddress` entry for each proxy, which means duplicating fields like `nodeCertHash`.

The new protobuf avoid this duplication, letting us represent node 0 in a protobuf equivalent of,

```
{
    "nodeId" : 0,
    "certHash" : "337390d8fea144afc12e81254a28dac6ea82893836ac072effd85e0a7748580ef28096648c5a7f8dbb4ce81476815137",
    "nodeAccount" : "0.0.3",
    "serviceEndpoints" : [ { 
      "ipAddressV4" : "13.82.40.153",
      "port" : 50211
    }, {
      "ipAddressV4" : "34.239.82.6",
      "port" : 50211
    }, {
      "ipAddressV4" : "35.237.200.180",
      "port" : 50211
    } ] 
}
```

However, Services will continue to populate the deprecated fields in duplicate entries for six months, to give all consumers of files `0.0.101` and `0.0.102` time to prepare for exclusive use of the new format. After six months, we will eliminate the duplication and the `ipAddress`, `portno`, and `memo` fields will be left empty. (The fields will never be removed to ensure it remains possible to parse early versions of these system files.)

In a minor point, Services now rejects any protobuf `string` field whose UTF-8 encoding includes the zero-byte character; that is, Unicode code point 0, `NUL`. Databases (for example, PostgreSQL) commonly reserve this character as a delimiter in their internal formats, so allowing it to occur in entity fields can make life harder for Mirror Node operators.

To simplify tasks for network admins, we have also streamlined the signing requirements for updates to system accounts, and introduced a Docker-based utility called "yahcli" for admin actions such as updating system files.

## [v0.12.0](https://github.com/hashgraph/hedera-services/releases/tag/v0.12.0-rc.2)

{% hint style="success" %}
**MAINNET UPDATE COMPLETED: MARCH 12, 2021**
{% endhint %}

{% hint style="success" %}
**TESTNET UPDATE COMPLETED: FEBRUARY 26, 2021**
{% endhint %}

In Hedera Services v0.12.0, we completed the MVP implementation of the Hedera Scheduled Transaction Service (HSTS) as detailed in [this](https://github.com/hashgraph/hedera-services/blob/master/docs/scheduled-transactions/spec.md) design document. This service decouples _what_ should execute on the ledger from _when_ it should execute, giving new flexibility and programmability to users. Note that HSTS operations are enabled on Previewnet, but remain disabled on Testnet and Mainnet at this time.

We have given users of the Hedera Token Service (HTS) more control over the lifecycle of their token associations. In v0.11.0, deleted tokens were immediately dissociated from all accounts. This automatic dissociation no longer occurs. If account `X` is associated with token `Y`, then even if token `Y` is marked for deletion, a `getAccountInfo` query for `X` will continue to show the association with `Y` \_until\_it is explicitly removed via a `tokenDissociateFromAccount` transaction. Note that for convenience, queries that return token balances now also return the `decimals` value for the relevant token. This allows a user to interpret e.g. `balance=10050` as `100.50` tokens given `decimals=2`.

In a final Hedera API (HAPI) change, we have extended the `memo` field present on contract and topic entities to the account, file, token, and scheduled transaction entities. (Note this `memo` is distinct from the short-lived `memo` that may be given to any `TransactionBody`for inclusion in the `TransactionRecord`.) All of these changes to HAPI are now more easily browsed via GitHub pages [here](https://hashgraph.github.io/hedera-protobufs/); the new [`hashgraph/hedera-protobufs` repository](https://github.com/hashgraph/hedera-protobufs) is now the authoritative source of the protobuf files defining HAPI.

Apart from these enhancements to HAPI, the "streams" consumable by mirror node operators now include an alpha version of a protobuf file that contains the same information as the `_Balances.csv` files. The type of this file is [`AllAccountBalances`](https://hashgraph.github.io/hedera-protobufs/#proto.AllAccountBalances).

## [v0.11.0](https://github.com/hashgraph/hedera-services/releases/tag/v0.11.0)

{% hint style="success" %}
**MAINNET UPDATE COMPLETED: FEBRUARY 4, 2021**
{% endhint %}

{% hint style="success" %}
**TESTNET UPDATE COMPLETED: JANUARY 26, 2021**
{% endhint %}

In Hedera Services v0.11.0, we upgraded the record stream format from v2 to v5 and the event stream format from v3 to v5. These changes are described in detail in the "Record and Event Stream File Formats" [article](https://docs.hedera.com/guides/docs/record-and-event-stream-file-formats).

We also updated startup code to make the number of system accounts in development and pre-production networks match the number of system accounts on mainnet, [creating](https://github.com/hashgraph/hedera-services/issues/784) account numbers `900-1000` on startup if they do not exist.

## [v0.10.0](https://github.com/hashgraph/hedera-services/releases/tag/v0.10.0)

{% hint style="success" %}
**MAINNET UPDATE COMPLETED: JANUARY 7, 2021**
{% endhint %}

{% hint style="success" %}
**TESTNET UPDATE COMPLETED: DECEMBER 17, 2020**
{% endhint %}

In Hedera Services v0.10.0, we improved the usability of the Hedera Token Service (HTS) with a `newTotalSupply` field in the receipts of `TokenMint` and `TokenBurn` transactions. Without this field, a client must follow the entire record stream of a token's supply changes to be certain of its supply at the consensus timestamp in the receipt. (Note that HTS operations are now enabled on Previewnet and Testnet, but remain disabled on Mainnet at this time. Please consult the [SDK documentation](https://docs.hedera.com/452354233115445331/token-service) for HTS semantics.)

Also for HTS, we added a property `fees.tokenTransferUsageMultiplier` that scales the resource usage assigned to a `CryptoTransfer` that changes token balances. This scaling factor is expected to be set so that the cost of a `CryptoTransfer` that changes two token balances is roughly 10x the cost of a `CryptoTransfer` that changes only two hbar balances.

Apart from HTS, this release drops a restriction on what payer accounts can be used for `CryptoUpdate` transactions that target system accounts. (That is, accounts with numbers not greater than `hedera.numReservedSystemEntities`.) In earlier versions, only three payers were accepted: The target account itself, the system admin account, or the treasury account. Other payers resulted in a status of `AUTHORIZATION_FAILED`. This entire restriction is removed, with one exception---the treasury must pay for a `CryptoUpdate` targeting the treasury.

Apart from these functional changes, we fixed an unintentional change in the naming of the crypto balances CSV file, and improved the usefulness of clients under _test-clients/_ for testing reconnect scenarios.

## [v0.9.0](https://github.com/hashgraph/hedera-services/releases/tag/v0.9.0-rc.1)

{% hint style="success" %}
**MAINNET UPDATE COMPLETED: DECEMBER 3, 2020**
{% endhint %}

{% hint style="success" %}
**TESTNET UPDATE COMPLETED: NOVEMBER 19, 2020**
{% endhint %}

In Hedera Services v0.9.0, we finished the alpha implementation of the Hedera Token Service (HTS). Note that all HTS operations are enabled on Previewnet, but remain disabled on Testnet and Mainnet. Please consult the [SDK documentation](https://docs.hedera.com/452354233115445331/token-service) for HTS semantics.

We made several changes to the HAPI protobuf. First, we removed the deprecated `SignatureList` message type. Second, we added a top-level `signedTransactionBytes` field to the `Transaction` message to ensure deterministic transaction hashes across different client libraries; the top-level `bodyBytes` and `sigMap` fields are now deprecated and the already-deprecated `body` field is removed. Third, we deprecated all fields related to non-payer records, include account send and receive thresholds. This followed from the effective removal of non-payer records in v0.8.1.

For the same reason, the semantics of the `CryptoGetRecords` and `ContractGetRecords` queries have also changed. The only queryable records are now those granted to the effective payer of a transaction that was handled while the network property `ledger.keepRecordsInState=true`. Such records have an expiry of 180 seconds. It is important to note that because a contract account can never be the effective payer for a transaction, any `ContractGetRecords` query will always return an empty record list, and we have deprecated the query.

## [v0.8.1](https://github.com/hashgraph/hedera-services/releases/tag/v0.8.1-rc1)

{% hint style="success" %}
**MAINNET UPDATE COMPLETED: OCTOBER 22, 2020**
{% endhint %}

{% hint style="success" %}
**TESTNET UPDATE COMPLETED: OCTOBER 7, 2020**
{% endhint %}

The mainnet release includes the 0.8.0 version updates.

## [v0.8.0](https://github.com/hashgraph/hedera-services/releases/tag/v0.8.0-rc1)

{% hint style="success" %}
**TESTNET UPDATE COMPLETED: SEPTEMBER 17, 2020**
{% endhint %}

In Hedera Services v0.8.0, we made several minor fixes and improvements. This tag also includes pre-release implementations of several operations for an incipient Hedera Token Service (HTS).

**NOTE:** HTS operations will remain disabled in non-development environments for some time. These operations are under active development; please consult `master` for up-to-date semantics.

### Enhancements

* Deprecated fields related to threshold records in HAPI protobuf  [#506](https://github.com/hashgraph/hedera-services/issues/506)
* Update Receipt proto to pair each Status with NodeID - Receipt is deleted only when the latest (duplicate) transaction expires. `getTxRecord` API will continue to return ALL records with the transaction ID.
* First drafts of `tokenCreate`, `tokenUpdate`, `tokenDelete`, `tokenTransfer`, `tokenFreeze`, `tokenUnfreeze`, `tokenGrantKyc`, `tokenRevokeYc`, `tokenWipe`, and `getTokenInfo` HAPI operations. [#505](https://github.com/hashgraph/hedera-services/pull/505) and [#522](https://github.com/hashgraph/hedera-services/pull/522)&#x20;

## [v0.7.0](https://github.com/hashgraph/hedera-services/releases/tag/v0.7.0-alpha1)

{% hint style="success" %}
**MAINNET UPDATE COMPLETED: SEPTEMBER 8, 2020**
{% endhint %}

{% hint style="success" %}
**TESTNET UPDATE COMPLETED: AUGUST 20, 2020**
{% endhint %}

In Hedera Services v0.7.0, we’ve moved to Swirlds SDK release `0.7.3` which enables zero-stake nodes to be part of a network without affecting consensus. Hedera Services v0.7.0 migrated to new interfaces and methods provided in this version of the Swirlds SDK. HCS topic running hashes are now calculated including the payer account id. The release includes other minor fixes and improvements.

**Enhancements**

* Migrate to Swirlds SDK release `0.7.3` with appropriate settings and logging configurations [#347](https://github.com/hashgraph/hedera-services/issues/347), [#427](https://github.com/hashgraph/hedera-services/issues/427)
* Update HCS topic running hash to include the payer account id [#88](https://github.com/hashgraph/hedera-services/issues/88)
* Add zero-stake node functionality [#274](https://github.com/hashgraph/hedera-services/issues/274)
* Add new stats for the average size of HCS submit message transactions that got handled and for counting the number of platform transactions not created per second [#316](https://github.com/hashgraph/hedera-services/issues/316), [#334](https://github.com/hashgraph/hedera-services/issues/334)
* Change gRPC CipherSuite to be CNSA compliant [#215](https://github.com/hashgraph/hedera-services/issues/215)
* Make recordLogPeriod dynamic with a default of 2 seconds [#315](https://github.com/hashgraph/hedera-services/issues/315)
* Add record with 3-min expiry to effective payer account after handling transaction [#348](https://github.com/hashgraph/hedera-services/issues/348)
* Enhancements for going open source [#378](https://github.com/hashgraph/hedera-services/issues/378), [#379](https://github.com/hashgraph/hedera-services/issues/379)

**Documentation changes**

* Clarify interpretation of response codes `UNKNOWN` and `PLATFORM_TRANSACTION_NOT_CREATED` [#314](https://github.com/hashgraph/hedera-services/issues/314), [#394](https://github.com/hashgraph/hedera-services/issues/394)

**Bug fixes**

* Prevent `CryptoCreate` and `CryptoUpdate` transactions from giving an account an empty key [#58](https://github.com/hashgraph/hedera-services/issues/58), [#60](https://github.com/hashgraph/hedera-services/issues/60)
* Fix incorrect submitted smart contract transactions count [#371](https://github.com/hashgraph/hedera-services/issues/371)
* Validate total ledger balance before starting up Services [#258](https://github.com/hashgraph/hedera-services/issues/258)
* Add a new rolling file to log all queries with controlled maximum rate [#59](https://github.com/hashgraph/hedera-services/issues/59)
* Other minor bugs [#373](https://github.com/hashgraph/hedera-services/issues/373)

## [v0.6.0](https://github.com/hashgraph/hedera-services/releases/tag/v0.6.0)

{% hint style="success" %}
**MAINNET UPGRADE COMPLETED: AUGUST 6, 2020**
{% endhint %}

{% hint style="success" %}
**TESTNET UPGRADE COMPLETED: JULY 16, 2020**
{% endhint %}

In Hedera Services v0.6.0, we’ve enhanced the Hedera Consensus Service by supporting [HCS Topic Fragmentation](https://github.com/hashgraph/hedera-services/issues/53). We added, into the `ConsensusSubmitMessageTransactionBody`, an optional field for the current chunk information. For every chunk, the payer account that is part of the `initialTransactionID` must match the Payer Account of this transaction. The entire `initialTransactionID` should match the `transactionID` of the first chunk, but this is not checked or enforced by Hedera except when the chunk number is 1.

**Enhancements**

* Add support for HCS Topic Fragmentation

**Documentation changes**

* Protobuf v0.6.0 with HAPI doc update to support HCS Topic Fragmentation&#x20;

## [**v0.5.8**](https://github.com/hashgraph/hedera-services/releases/tag/oa-release-r5-rc8)

{% hint style="success" %}
**MAINNET UPGRADE COMPLETED: JUNE 18, 2020**

v0.5.8 includes all of the updates found in [v0.5.0](services.md#v-0-5-0)
{% endhint %}

{% hint style="success" %}
**TESTNET UPGRADE COMPLETED: JUNE 8, 2020**
{% endhint %}

Version 0.5.8 includes a patch which addresses the resilience of peer-to-peer networking in the hashgraph consensus platform.

## **v0.5.0**

{% hint style="success" %}
**TESTNET UPGRADE COMPLETED: MAY 5, 2020**
{% endhint %}

In Hedera Services v0.5.0, we’ve added TLS for trusted communication with nodes on the Hedera network. For better security, only TLS v1.2 and v1.3 with TLS\_ECDHE\_ECDSA\_WITH\_AES\_256\_GCM\_SHA384 and TLS\_RSA\_WITH\_AES\_256\_GCM\_SHA384 cipher suites are allowed.

We’ve added new metadata in the Hedera NodeAddressBook, accessible in system file 0.0.101. The versions of the node software and gRPC Hedera API (HAPI) are now queryable via GetVersionInfo under the new NetworkService for node and network-scoped operations.

For Hedera Consensus Service, we’ve updated the topic running hash calculation to use the SHA-384 hash of the submitted message, rather than the message itself. This reduces the storage requirements needed to validate the hash of a topic. The record of a ConsensusSubmitMessage transaction that uses the new hashing scheme will have a new topicRunningHashVersion field in its receipt. The value of the field will be 2.

Hedera File Service also has several fixes of note. First, we enabled immutable files. Second, we relaxed the signing requirements for a FileDelete transaction to match the semantics of a revocation service. Third, we fixed a fee calculation bug that overcharged certain FileUpdate transactions.

For Hedera Smart Contract Service, we’ve improved visibility into transactions that create child contracts using the new keyword by putting created ids in the record of the transaction; and we now propagate parent contract metadata to created children.

Finally, if you use the throttle properties in system file 0.0.121 to estimate network performance limits, you will also be interested in a new standardized format of those properties. The lists below contain these and other minor updates, bug fixes, and documentation changes.

**Enhancements**

* Add support for TLS
* Expand address book metadata
* Return all created contract ids&#x20;
* Propagate creator contract metadata
* Introduce GetVersionInfo query&#x20;
* Standardize throttle configuration&#x20;
* Enforce file.encoding=utf-8 on startup&#x20;
* Make duration properties inclusive for readability

**Bug fixes**

* Use message SHA-384 hash in running hash&#x20;
* Enable immutable files &#x20;
* Relax FileDelete signing requirements&#x20;
* Fix sbh calculation in FileUpdate&#x20;
* Return metadata for deleted files
* Enforce receiver signing requirements during contract execution&#x20;
* Reject invalid CryptoGetInfo
* Reject CryptoCreate with empty key
* Return NOT\_SUPPORTED for state proof queries&#x20;
* Waive fees for 0.0.57 updating 0.0.111
* Waive signing requirements for 0.0.55 updating 0.0.121/0.0.122
* Waive all fees for 0.0.2
* Do not throttle system accounts&#x20;

**Documentation changes**

* Replace “claim” with “livehash” as appropriate
* Standardize and clarify HAPI doc

## v0.4.1

* Software update includes the ability for Hedera to dynamically set throttles on network transaction types.&#x20;
* The following throttles would be updated to: 1000 submit messages per second and 5 topic creates per second.&#x20;
* Reassigning of new Council Member nodes&#x20;

## v0.4.0

* Say hello to the Hedera Consensus Service! This release is the first to include HCS, allowing verifiable timestamping and ordering of application messages. &#x20;
* Network pricing has been updated to include HCS transactions and queries&#x20;
* Network throttle for HCS set to 1000 tps for submitting messages, and 100 tps for each of the other HCS operations.&#x20;
* Improved end to end testing.
* General code clean up and refactoring.
* ContractCall - TransactionReceipt response to ContractCall no longer includes the contractID called
* CryptoUpdate - TransactionReceipt response to CryptoUpdate no longer includes the accountID updated
* CryptoTransfer – CryptoTransfer transactions resulting in INSUFFICIENT\_ACCOUNT\_BALANCE error no longer list Transfers in the TransactionRecord transferList that were not applied

### Miscellaneous

### SDKs

* Java SDK has been updated to support the Hedera Consensus Service
* JavaScript/Typescript SDK has reached version 1.0.0, supporting all four mainnet services
* JavaScript/Typescript SDK supports both running in the browser (with Envoy Proxy) and in Node.
* Go SDK now supports all four mainnet services.

**Fees**

* Transfer list within transaction records now shows only a single net amount in or out for each account, reflecting both transfers and any fees paid.
* Fixed bug in fee schedule that had resulted in fees for ContractCallLocal, ContractGetBytecode, and getVersion queries being undercharged by \~33%
* You may get more information regarding transaction record fees [here](https://docs.hedera.com/guides/mainnet/fees/transaction-records).

### SDK Extension Components

* The Hedera SDK Extension Components (SXC) is an open sourced set of pre-built components that aim to provide additional functionality over and above HCS to make it easier and quicker to develop applications, particularly if they require secure communications between participants.
* Components use the Hedera Java SDK to communicate with the Hedera Consensus Service.
* Learn more about Hedera SXC [here](https://github.com/hashgraph/hedera-hcs-sxc).
