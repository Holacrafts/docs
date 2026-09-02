# Holacrafts

Holacrafts is a **local-first workspace built around user-owned Spaces, structured Pages, and private end-to-end encrypted collaboration**.

The architecture follows a simple principle: **users remain in control of their data, access, and workspace**.

Data already stored on a user's device remains locally accessible in the Holacrafts app without requiring continuous access to Holacrafts servers. Protected Page content is encrypted on the client before synchronization and decrypted on authorized clients.

Holacrafts servers support synchronization, delivery, sharing, publication, and other network functions. They are not the primary runtime for a user's local workspace.

**Your work stays yours — organized around your needs, private by default, and shared on your terms.**

## Core model

```text
Space
  └── Page
       └── Elements
```

- **Space** is the main user-controlled workspace.
- **Page** is a structured entity inside a Space.
- **Elements** are the building blocks of a Page.

The client is responsible for local Page state, rendering, cryptographic operations, and lifecycle behavior. Remote services participate where network interaction is required, including synchronization, sharing, invitations, and public publication.

## Architectural principles

**Local-first.** Data already stored on your device remains available to Holacrafts without requiring continuous server access.

**Privacy and end-to-end encryption.** Protected content is encrypted on the client before synchronization. Private sharing between established Holacrafts contacts uses end-to-end encrypted key exchange.

**First contact.** An initial invitation to a new contact uses a temporary access key to establish the connection. Subsequent private sharing uses end-to-end encryption.

**User-controlled access.** Users choose when and how to share Pages. Private sharing, recipient-specific access, and explicitly created Public Links are distinct access modes. Public publication is an intentional user action, not the default state of user content.

**Structured content.** Pages are composed from explicit Elements rather than being treated only as arbitrary documents or files.

**Separated responsibilities.** Local runtime, cryptography, synchronization, access, messaging, Activity, and publication are separate parts of the system rather than a single server-controlled document lifecycle.

## Documentation

- [Architecture](ARCHITECTURE.md) — the public high-level architecture and client/server boundary.
- [Security](SECURITY.md) — the public security, privacy, and encryption model.
- [Threat Model](THREAT_MODEL.md) — the security boundaries, assumptions, and limitations of the model.

This repository contains public technical documentation for Holacrafts. It is not the source repository for the Holacrafts application.
