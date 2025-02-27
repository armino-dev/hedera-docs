# Signature

{% hint style="info" %}
This message is deprecated and succeeded by [SignaturePair](signature-pair.md) and [SignatureMap ](signaturemap.md)messages.
{% endhint %}

A Signature corresponding to a Key. It is a sequence of bytes holding a public key signature from one of the three supported systems (ed25519, RSA-3072, ECDSA with p384). Or, it can be a list of signatures corresponding to a single threshold key. Or, it can be the ID of a smart contract instance, which is authorized to act as if it had a key. If an account has an ed25519 key associated with it, then the corresponding private key must sign any transaction to transfer cryptocurrency out of it. If it has a smart contract ID associated with it, then that smart contract is allowed to transfer cryptocurrency out of it. The smart contract doesn't actually have a key, and doesn't actually sign a transaction. But it's as if a virtual transaction were created, and the smart contract signed it with a private key. A key can also be a "threshold key", which means a list of M keys, any N of which must sign in order for the threshold signature to be considered valid. The keys within a threshold signature may themselves be threshold signatures, to allow complex signature requirements (this nesting is not supported in the currently, but will be supported in a future version of API). If a Signature message is missing the "signature" field, then this is considered to be a null signature. That is useful in cases such as threshold signatures, where some of the signatures can be null.

| Field      | Type               | Description                                   | ​                                                                                                                                      |
| ---------- | ------------------ | --------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| deprecated | ​                  | ​                                             | ​                                                                                                                                      |
| signature  | oneof              | ​                                             | ​                                                                                                                                      |
| ​          | contract           | ​                                             | Smart contract virtual signature (always length zero)                                                                                  |
| ​          | ed25519            | ​                                             | Ed25519 signature bytes                                                                                                                |
| ​          | RSA\_3072          | ​                                             | RSA-3072 signature bytes                                                                                                               |
| ​          | ECDSA\_384         | ​                                             | ECDSA p-384 signature bytes                                                                                                            |
| ​          | thresholdSignature | ​[ThresholdSignature](thresholdsignature.md)​ | A list of signatures for a single N-of-M threshold Key. This must be a list of exactly M signatures, at least N of which are non-null. |
| ​          | signatureList      | ​[SignatureList](signature-list.md)​          | A list of M signatures, each corresponding to a Key in a KeyList of the same length.                                                   |

#### &#x20; <a href="#undefined" id="undefined"></a>
