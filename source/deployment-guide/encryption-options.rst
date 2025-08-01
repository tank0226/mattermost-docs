Encryption options
==================

.. include:: ../_static/badges/allplans-selfhosted.rst
  :start-after: :nosearch:

Mattermost provides encryption-in-transit and encryption-at-rest capabilities. This page guides you through setting up appropriate encryption security.

Encryption is not required for GDPR, although it can be used as an additional safeguard against data breach.

Encryption-in-transit
---------------------

Mattermost supports TLS encryption including AES-256 with 2048-bit RSA on all data transmissions between Mattermost client applications and the Mattermost server. You may either set up TLS on the Mattermost Server or install a proxy such as NGINX and set up TLS on the proxy. Refer to our :doc:`configuration guide for more details </deployment-guide/server/setup-tls>`.

Connections to Active Directory/LDAP can :ref:`optionally be secured with TLS or stunnel <administration-guide/configure/authentication-configuration-settings:ad/ldap port>`.

Connections to calls are secured with a combination of:

  - TLS: The existing WebSocket channel is used to secure the signaling path.
  - DTLS v1.2 (mandatory): Used for initial key exchange. Supports ``TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256`` and ``TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA`` algorithms.
  - SRTP (mandatory): Used to encrypt all media packets (i.e. those containing voice or screen share). Supports ``AEAD_AES_128_GCM`` and ``AES128_CM_HMAC_SHA1_80`` algorithms.

Gossip encryption (experimental)
--------------------------------

In a High Availability mode, Mattermost supports encryption of cluster data in-transit when using the gossip protocol, which is based on principles outlined in the `SWIM protocol developed by researchers at Cornell University <https://www.cs.cornell.edu/projects/Quicksilver/public_pdfs/SWIM.pdf>`_. The gossip protocol is a communication mechanism in distributed systems where nodes randomly exchange information to ensure data consistency across the network. It is decentralized, scalable, and fault-tolerant, making it ideal for systems with numerous nodes. Information is spread in a manner similar to social gossip, with nodes periodically "gossiping" updates to random peers until the network converges to a consistent state. Widely used in distributed databases, blockchain networks, and peer-to-peer systems, the protocol is simple to implement and resilient to node failures. However, it can suffer from redundancy and propagation delays in large networks.

The encryption uses AES-256 by default, and it is not configurable. However, it is possible to manually set the value in the ``Systems`` table for the ``ClusterEncryptionKey`` row. A key is a byte array converted to base64. It can be set to a length of 16, 24, or 32 bytes to select AES-128, AES-192, or AES-256 respectively.

Encryption-at-rest
------------------

Encryption-at-rest is available for messages via hardware and software disk encryption solutions applied to the Mattermost database, which resides on its own server within your infrastructure. To enable end user search and compliance reporting of message histories, Mattermost does not offer encryption within the database.

Both the database and file storage can be secured using software or hardware disk encryption solutions. Full-disk encryption methods such as LUKS (Linux), BitLocker (Windows), or database-specific solutions like Transparent Data Encryption (TDE) can be employed. Integration with external encrypted storage systems is supported.

Additionally, encryption-at-rest is available for files stored via hardware and software disk encryption solutions applied to the server used for local storage or storage via MinIO.

Database
~~~~~~~~

Encryption-at-rest is available for messages via hardware and software disk encryption solutions applied to the Mattermost database, which resides on its own server within your infrastructure. See the `PostgreSQL <https://www.postgresql.org/docs/10/encryption-options.html>`__ database documentation for details on encryption options at the disk level.

File storage
~~~~~~~~~~~~~

For local storage or storage via Minio, encryption-at-rest is available for files stored via hardware and software disk encryption solutions applied to the server.

For Amazon’s proprietary S3 system, encryption-at-rest is available via :ref:`server-side encryption with Amazon S3-managed keys <administration-guide/configure/environment-configuration-settings:enable server-side encryption for amazon s3>` in Mattermost enterprise-badge.

SAML encryption support
-----------------------

Mattermost supports the following encryption methods for SAML:

* aes128-gcm
* aes192-gcm
* aes256-gcm
* aes128-cbc
* aes256-cbc