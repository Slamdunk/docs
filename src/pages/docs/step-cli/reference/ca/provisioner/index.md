---
layout: auto-doc
title: step ca provisioner
menu:
  docs:
    parent: step ca
    children:
      - list
      - jwe-key
      - add
      - update
      - remove
---

## Name
**step ca provisioner** -- create and manage the certificate authority provisioners

## Usage

```raw
step ca provisioner <subcommand> [arguments] [global-flags] [subcommand-flags]
```

## Description

**step ca provisioner** command group provides facilities for managing the
certificate authority provisioners.

A provisioner is an entity that controls provisioning credentials, which are
used to generate provisioning tokens.

Provisioning credentials are simple JWK key pairs using public-key cryptography.
The public key is used to verify a provisioning token while the private key is
used to sign the provisioning token.

Provisioning tokens are JWT tokens signed by the JWK private key. These JWT
tokens are used to get a valid TLS certificate from the certificate authority.
Each provisioner is able to manage a different set of rules that can be used to
configure the bounds of the certificate.

In the certificate authority, a provisioner is configured with a JSON object
with the following properties:

* **name**: the provisioner name, it will become the JWT issuer and a good
  practice is to use an email address for this.
* **type**: the provisioner type, currently only "jwk" is supported.
* **key**: the JWK public key used to verify the provisioning tokens.
* **encryptedKey** (optional): the JWE compact serialization of the private key
  used to sign the provisioning tokens.
* **claims** (optional): an object with custom options for each provisioner.
  Options supported are:
  * **minTLSCertDuration**: minimum duration of a certificate, set to 5m by
    default.
  * **maxTLSCertDuration**: maximum duration of a certificate, set to 24h by
    default.
  * **defaultTLSCertDuration**: default duration of the certificate, set to 24h
    by default.
  * **disableRenewal**: whether or not to disable certificate renewal, set to false
    by default.

## Examples

List the active provisioners:
```shell
$ step ca provisioner list
```

Retrieve the encrypted private jwk for the given kid:
```shell
$ step ca provisioner jwe-key 1234 --ca-url https://127.0.0.1 --root ./root.crt
```

Add a single provisioner:
```shell
$ step ca provisioner add max@smallstep.com max-laptop.jwk --ca-config ca.json
```

Remove the provisioner matching a given issuer and kid:
```shell
$ step ca provisioner remove max@smallstep.com --kid 1234 --ca-config ca.json
```

## Commands


| Name | Usage |
|---|---|
| **[list](list/)** | list provisioners configured in the CA |
| **[jwe-key](jwe-key/)** | retrieve and print a provisioning key in the CA |
| **[add](add/)** | add a provisioner |
| **[update](update/)** | update a provisioner |
| **[remove](remove/)** | remove a provisioner from the CA configuration |

