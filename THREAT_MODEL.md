# Holacrafts Threat Model

## 1. Security Goal

**Holacrafts is designed to keep users in control of their data.**

The security model combines three properties:

- **Local-first possession** — data already stored on a user's device remains locally available to the Holacrafts app without requiring continuous access to Holacrafts servers.
- **End-to-end encryption** — protected content is encrypted on the client before synchronization and decrypted on authorized clients.
- **User-controlled access** — users decide whether content remains private, is shared with intended recipients, or is explicitly published.

Holacrafts infrastructure supports synchronization and other network services without becoming the primary plaintext runtime for protected user content.

Server availability is still required for network-dependent operations such as synchronization, remote sharing, invitations, and publication.

## 2. Local-first Boundary

Data already stored locally remains available to the Holacrafts application without first retrieving that content from Holacrafts servers.

A server outage therefore does not, by itself, remove the user's local copy or prevent locally available data from being opened in the app.

Local-first does not mean that every Holacrafts function works without a network connection. Synchronization and other remote operations still depend on the corresponding services.

Local device storage is part of the trusted endpoint environment. The threat model for synchronized protected content and the threat model for a compromised authorized device are therefore separate.

## 3. End-to-End Encryption

Protected Page content, Elements, messages, and attachments are encrypted on the client before synchronization.

For established Holacrafts contacts, private sharing uses recipient-specific end-to-end encrypted key exchange. The private cryptographic keys required for that exchange remain with the participating clients.

Holacrafts infrastructure can store and relay encrypted protected content without requiring plaintext access to that content.

Encryption and access are separate concerns: granting cryptographic access to another recipient does not require making the protected content public.

## 4. First-contact Bootstrap

A first invitation to a new contact has a different trust boundary from subsequent private sharing.

To establish the initial connection, a temporary access key is relayed through the invitation delivery path. Holacrafts does not retain that key as a persistent server-side secret.

Because the initial access capability is delivered by email, the security of that first delivery depends in part on the security of the recipient's email provider.

Once the Holacrafts contact is established, subsequent private sharing uses end-to-end encrypted key exchange between the participating clients.

This bootstrap distinction applies to the initial relationship establishment and does not change the fact that the protected Page content itself is encrypted before synchronization.

## 5. Service-provider Access

Holacrafts treats its infrastructure as outside the intended plaintext boundary for protected user content.

Protected Page content remains encrypted across the Holacrafts service boundary.

## 6. Server Compromise

A compromise of Holacrafts infrastructure remains a serious security event, but should not automatically become a compromise of the plaintext protected content synchronized through the service.

An attacker may obtain encrypted user content and server-visible service metadata.

For established-contact E2EE sharing, server-side data alone does not provide the private client-side cryptographic keys required to complete the recipient key exchange.


## 7. Data Interception

Protected content is encrypted at the application layer before synchronization.

The confidentiality model therefore does not rely only on transport security between a Holacrafts client and Holacrafts infrastructure.

Transport security and end-to-end encryption serve different purposes: transport security protects the network connection, while application-layer encryption protects the content across the service boundary.


## 8. Access Modes

Holacrafts separates the encrypted content model from the user's chosen access mode.

### Private

Private content remains available to its owner and is not shared with another recipient.

### Invite / Shared

A Page can be shared with an intended recipient without making it public.

Established Holacrafts contacts use recipient-specific end-to-end encrypted key exchange.

### Public Link

Public-link access is a separate and explicit user action.

The client creates the cryptographic material required for public access. The secret required to open the encrypted content is carried with the complete link shared by the user and is not provided to Holacrafts servers as part of the server-side Page data.

Holacrafts infrastructure can therefore serve encrypted published content without possessing the complete cryptographic capability represented by the shared link.

**Public means intentionally shareable by the user, not plaintext-readable by Holacrafts.**

Anyone who receives the complete Public Link may obtain the access it conveys.

## 9. Recipient Trust and Revocation

End-to-end encryption controls the delivery of cryptographic access. It cannot control what an authorized recipient does with plaintext after legitimately decrypting it.

A recipient may be able to copy, export, record, screenshot, or otherwise retain information they were authorized to view.

Private access links and Public Links can be closed through Holacrafts. Closing access prevents continued retrieval through the corresponding Holacrafts server-side access path.


## 10. Device Compromise

An authorized endpoint must eventually decrypt protected content in order to present it to the user.

A fully compromised authorized device may therefore expose plaintext content and cryptographic material available to that device.

End-to-end encryption protects content across the service boundary; it does not by itself protect an authorized endpoint that is already under an attacker's control.


## 11. Metadata

End-to-end encryption protects protected user content, not every piece of information required to operate the service.

Holacrafts infrastructure may process service metadata required for functions such as accounts, authentication, routing, synchronization, sharing, delivery, notifications, and publication.

This metadata is operationally distinct from the protected Page content encrypted by the client.

## 12. What This Model Does Not Protect Against

The Holacrafts security model does not make every endpoint, recipient, or external delivery channel trustworthy.

In particular, it does not by itself prevent:

- an authorized recipient from retaining content after receiving it;
- a user from intentionally sharing a complete Public Link;
- exposure of plaintext or cryptographic material on a fully compromised authorized endpoint;
- loss or compromise caused by security properties outside the verified Holacrafts encryption boundary.

Likewise, local-first storage protects the user's possession of data already stored on the device; it does not make network-dependent functionality independent of Holacrafts infrastructure.

## 13. Security Invariants

The following principles define the intended Holacrafts security boundary:

1. **Protected content is encrypted on the client before synchronization.**
2. **Protected content is decrypted on authorized clients.**
3. **Cryptographic operations for protected content are performed client-side.**
4. **Holacrafts servers do not require plaintext protected content to provide storage and synchronization.**
5. **Private sharing between established Holacrafts contacts uses recipient-specific end-to-end encrypted key exchange.**
6. **Private client keys required for established-contact key exchange remain client-side.**
7. **A first invitation uses a temporary bootstrap access key that is relayed for delivery and is not retained as a persistent Holacrafts server secret.**
8. **Data already stored locally does not require Holacrafts servers merely to remain locally accessible in the application.**
9. **Private, Invite / Shared, and Public Link are distinct access modes.**
10. **Sharing with an intended recipient does not automatically make content public.**
11. **Public-link access does not require storing the complete link decryption capability as part of the server-side Page data.**
12. **Publishing and sharing access are user-controlled actions.**

## Scope Note

This document describes the security properties and threat boundaries of the Holacrafts architecture as currently established.

Implementation-specific guarantees should be documented only after the corresponding behavior has been inspected and verified.
