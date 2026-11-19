# Amazon N2 root certificates

This directory holds the root CA certificates for Amazon N2 certificates issued by AWS Certificate Manager (ACM). Add these roots into the trust store of any client or server that must verify an Amazon N2 certificate.

## Intended use

Amazon N2 certificates are for service-to-service TLS and mutual TLS (mTLS), where you control the trust store on both sides.

- **Not for browsers.** Do not use Amazon N2 certificates for websites that browsers visit. The Amazon N2 roots are not in browser trust stores.
- **Not logged to Certificate Transparency.** Amazon N2 certificates are not submitted to public CT logs, so browsers that require CT reject them, including those in the `N2_SFG2_AMAZON_MANAGED` hierarchy that chain to Starfield G2.

## Hierarchies

ACM issues Amazon N2 certificates from two root hierarchies. You choose one with the `RootHierarchyType` parameter of `RequestCertificate`.

| `RootHierarchyType` | Roots | Add to your trust store |
|---|---|---|
| `N2_AMAZON_MANAGED` | Amazon N2 RSA 2048 Root M1, Amazon N2 ECDSA 256 Root M1, Amazon N2 ECDSA 384 Root M1, Amazon N2 RSA 4096 Root M1 | All four Amazon N2 roots from `roots/` |
| `N2_SFG2_AMAZON_MANAGED` | Amazon SFG2 N2 RSA 2048 Root M1, Amazon SFG2 N2 ECDSA 256 Root M1, Amazon SFG2 N2 ECDSA 384 Root M1, Amazon SFG2 N2 RSA 4096 Root M1 | All four Amazon SFG2 N2 roots from `roots/`, or Starfield Services Root Certificate Authority - G2 |

The two RSA 4096 roots do not issue certificates today.

The Amazon SFG2 N2 RSA 2048, ECDSA 256 and ECDSA 384 roots also have a certificate issued by Starfield Services Root Certificate Authority - G2. These cross-signed certificates are in `cross-signed-roots/`. They carry the same public key as the self-signed root, so a client that trusts either one verifies the same chains. Cross-signed certificates are intermediates, not trust anchors: do not add them to a trust store.

## Files

| Path | Contents |
|---|---|
| `roots/pem/`, `roots/der/` | The eight self-signed roots, PEM (`.pem`) and DER (`.cer`) |
| `cross-signed-roots/pem/`, `cross-signed-roots/der/` | The three Starfield G2 cross-signed certificates (`…-Cross-Signed`) |

| Root | Valid until |
|---|---|
| Amazon N2 RSA 2048 Root M1 | 2046-08-28 |
| Amazon N2 ECDSA 256 Root M1 | 2046-08-28 |
| Amazon N2 ECDSA 384 Root M1 | 2046-08-28 |
| Amazon N2 RSA 4096 Root M1 | 2046-08-28 |
| Amazon SFG2 N2 RSA 2048 Root M1 (self-signed) | 2036-08-30 |
| Amazon SFG2 N2 ECDSA 256 Root M1 (self-signed) | 2036-08-30 |
| Amazon SFG2 N2 ECDSA 384 Root M1 (self-signed) | 2036-08-30 |
| Amazon SFG2 N2 RSA 4096 Root M1 (self-signed) | 2036-08-30 |
| Amazon SFG2 N2 roots, cross-signed by Starfield G2 | 2037-12-30 |

## Verify a file

Compute the SHA-256 certificate fingerprint:

```sh
openssl x509 -in roots/pem/Amazon-N2-RSA-2048-Root-M1.pem -noout -fingerprint -sha256
```

Compute the SHA-256 hash of the public key (SPKI). If you must pin, pin to this value for the root; see the pinning guidance in the repository README:

```sh
openssl x509 -in roots/pem/Amazon-N2-RSA-2048-Root-M1.pem -noout -pubkey \
  | openssl pkey -pubin -outform DER \
  | openssl dgst -sha256 -binary | base64
```

Check that a cross-signed certificate was issued by Starfield G2. `SFSRootCAG2.pem` is available from https://www.amazontrust.com/repository/:

```sh
openssl verify -partial_chain -CAfile SFSRootCAG2.pem \
  cross-signed-roots/pem/Amazon-SFG2-N2-RSA-2048-Root-M1-Cross-Signed.pem
```

## Provenance

Amazon Trust Services generated the self-signed roots at a key ceremony on 2026-09-02 and issued the Starfield G2 cross-signed certificates at a key ceremony on 2026-09-28. The files here are the certificates produced by those ceremonies, unchanged.
