# Adversarial Model for Agent-Driven Attestation in Keylime

_This is a supplement to the current [enhancement proposal](https://github.com/keylime/enhancements/issues/103) for adding support for agent-driven attestation to Keylime._

In the design of security protocols, it is prudent to define a threat model in terms of the capabilities of an idealised attacker. This has a number of advantages, not limited to the following:

- users are clear on the security properties they can expect from the system;
- developers have agreement on which attacks are in scope and which are out of scope; and
- the protocols naturally lend themselves to analysis by outside parties.

In lieu of a full formal model, we give a plain English description, translatable to formal definitions, in the subsections below.

## Security Goals

We give the main security property for Keylime by stating what a successful adversary must achieve:

> A valid attack against Keylime is one in which an adversary can cause a mismatch between a verification outcome reported by a verifier and the correct, expected verification outcome for the verified node.

This includes attacks in which:

- verification of a node is reported as having passed when the policy for the node should have resulted in a verification failure; or
- verification of a node is reported as having failed when the policy for the node should have resulted in a successful verification.

The latter is important to consider because, depending on how Keylime is used (e.g., if Keylime results are consumed by SPIRE or otherwise used for authentication of non-person entities), this could be exploited to trigger cascading failures throughout the network.

## The Capabilities of the Adversary

For our adversary, we consider a typical network-based (Dolev-Yao) attacker[^dolevyoa] which exercises full control over the network and can intercept, block and modify all messages but cannot break cryptographic primitives (all cryptography is assumed perfect). Because we need to consider attacks in which the adversary is resident on a node to be verified, we extend the "network" to include the channel between the agent and the TPM. It is assumed the adversary has full access to the filesystem and memory of the node. The adversary cannot corrupt (i.e., take control of, or impersonate) the TPM, boot firmware, verifier, registrar or tenant.

## Exclusions

Attacks which exploit poorly-defined verification policies or deficiencies in the information which can be obtained from IMA and UEFI logs (and other measures of system state) are necessarily out of scope. Additionally, we exclude attacks which are made possible by incorrect configuration by the user. Attacks which rely on UEFI bootkits or otherwise require the adversary to modify the measured boot process are also excluded.

<br><br>

**Footnotes**

[^dolevyoa]: This type of rule-based adversary is first described by Danny Dolev and Andrew Yao in their 1983 paper, ["On the security of public key protocols"](http://www.cs.huji.ac.il/~dolev/pubs/dolev-yao-ieee-01056650.pdf).