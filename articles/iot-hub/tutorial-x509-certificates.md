---
title: Tutorial - Understand X.509 public key certificates for Azure IoT Hub| Microsoft Docs
description: Tutorial - Understand X.509 public key certificates for Azure IoT Hub
author: kgremban

ms.service: iot-hub
services: iot-hub
ms.topic: tutorial
ms.date: 12/30/2022
ms.author: kgremban
ms.custom: [mvc, 'Role: Cloud Development', 'Role: Data Analytics']
#Customer intent: As a developer, I want to be able to use X.509 certificates to authenticate devices to an IoT hub. This step of the tutorial needs to introduce me to X.509 Public Key certificates.
---

# Tutorial: Understanding X.509 Public Key Certificates

X.509 certificates are digital documents that represent a user, computer, service, or device. They're issued by a certification authority (CA), subordinate CA, or registration authority and contain the public key of the certificate subject. They don't contain the subject's private key, which must be stored securely. Public key certificates are documented by [RFC 5280](https://tools.ietf.org/html/rfc5280). They're digitally signed and, in general, contain the following information:

* Information about the certificate subject
* The public key that corresponds to the subject's private key
* Information about the issuing CA
* The supported encryption and/or digital signing algorithms
* Information to determine the revocation and validity status of the certificate

## Certificate fields

Over time there have been three certificate versions. Each version adds fields to the one before. Version 3 is current and contains version 1 and version 2 fields in addition to version 3 fields. Version 1 defined the following fields:

* **Version**: A value (1, 2, or 3) that identifies the version number of the certificate
* **Serial Number**: A unique number for each certificate issued by a CA
* **CA Signature Algorithm**: Name of the algorithm the CA uses to sign the certificate contents
* **Issuer Name**: The distinguished name (DN) of the certificate's issuing CA
* **Validity Period**: The time period for which the certificate is considered valid
* **Subject Name**: Name of the entity represented by the certificate
* **Subject Public Key Info**: Public key owned by the certificate subject

Version 2 added the following fields containing information about the certificate issuer. These fields are, however, rarely used.

* **Issuer Unique ID**: A unique identifier for the issuing CA as defined by the CA
* **Subject Unique ID**: A unique identifier for the certificate subject as defined by the issuing CA

Version 3 certificates added the following extensions:

* **Authority Key Identifier**: This extension can be set to one of two values:
  * The subject of the CA and serial number of the CA certificate that issued this certificate
  * A hash of the public key of the CA that issued this certificate
* **Subject Key Identifier**: Hash of the current certificate's public key
* **Key Usage** Defines the service for which a certificate can be used. This extension can be set to one or more of the following values:
  * **Digital Signature**
  * **Non-Repudiation**
  * **Key Encipherment**
  * **Data Encipherment**
  * **Key Agreement**
  * **Key Cert Sign**
  * **CRL Sign**
  * **Encipher Only**
  * **Decipher Only**
* **Private Key Usage Period**: Validity period for the private key portion of a key pair
* **Certificate Policies**: Policies used to validate the certificate subject
* **Policy Mappings**: Maps a policy in one organization to policy in another
* **Subject Alternative Name**: List of alternate names for the subject
* **Issuer Alternative Name**: List of alternate names for the issuing CA
* **Subject Dir Attribute**: Attributes from an X.500 or LDAP directory
* **Basic Constraints**: Allows the certificate to designate whether it's issued to a CA, or to a user, computer, device, or service. This extension also includes a path length constraint that limits the number of subordinate CAs that can exist.
* **Name Constraints**: Designates which namespaces are allowed in a CA-issued certificate
* **Policy Constraints**: Can be used to prohibit policy mappings between CAs
* **Extended Key Usage**: Indicates how a certificate's public key can be used beyond the purposes identified in the **Key Usage** extension
* **CRL Distribution Points**: Contains one or more URLs where the base certificate revocation list (CRL) is published
* **Inhibit anyPolicy**: Inhibits the use of the **All Issuance Policies** OID (2.5.29.32.0) in subordinate CA certificates
* **Freshest CRL**: Contains one or more URLs where the issuing CA's delta CRL is published
* **Authority Information Access**: Contains one or more URLs where the issuing CA certificate is published
* **Subject Information Access**: Contains information about how to retrieve more details for a certificate subject

## Certificate formats

Certificates can be saved in various formats. Azure IoT Hub authentication typically uses the Privacy-Enhanced Mail (PEM) and Personal Information Exchange (PFX) formats.

### Binary certificate

A raw form binary certificate using Distinguished Encoding Rules (DER) ASN.1 encoding.

### ASCII PEM format

A PEM certificate (.pem) file contains a Base64-encoded certificate beginning with `-----BEGIN CERTIFICATE-----` and ending with `-----END CERTIFICATE-----`. One of the most common formats for X.509 certificates, PEM format is required by IoT Hub when uploading certain certificates.

### ASCII PEM key

Contains a Base64-encoded DER key, optionally with more metadata about the algorithm used for password protection.

### PKCS #7 certificate

A format designed for the transport of signed or encrypted data. It's defined by [RFC 2315](https://tools.ietf.org/html/rfc2315). It can include the entire certificate chain.

### PKCS #8 key

The format for a private key store defined by [RFC 5208](https://tools.ietf.org/html/rfc5208).

### PKCS #12 key and certificate

A complex format that can store and protect a key and the entire certificate chain. It's commonly used with a .pfx extension. PKCS #12 is synonymous with the PFX format.

## For more information

For more information, see the following articles:

* [The layman’s guide to X.509 certificate jargon](https://techcommunity.microsoft.com/t5/internet-of-things/the-layman-s-guide-to-x-509-certificate-jargon/ba-p/2203540)
* [Understand how X.509 CA certificates are used in IoT](./iot-hub-x509ca-concept.md)

## Next steps

If you want to generate test certificates that you can use to authenticate devices to your IoT Hub, see the following articles:

* [Tutorial: Using Microsoft-supplied scripts to create test certificates](tutorial-x509-scripts.md)
* [Tutorial: Using OpenSSL to create test certificates](tutorial-x509-openssl.md)
* [Tutorial: Using OpenSSL to create self-signed certificates](tutorial-x509-self-sign.md)

If you have a certification authority (CA) certificate or subordinate CA certificate and you want to upload it to your IoT hub and prove that you own it, see [Tutorial: Proving possession of a CA certificate](tutorial-x509-prove-possession.md).