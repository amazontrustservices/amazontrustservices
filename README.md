# Amazon Trust Services

This repository publishes the public certificates of Amazon Trust Services (ATS), the certificate authority operated by Amazon. The same certificates are available at https://www.amazontrust.com/repository/.

| Folder | Contents | Trusted by |
|---|---|---|
| [`web-pki/`](web-pki/) | Amazon Root CA 1–4, Starfield Services Root Certificate Authority - G2, and their cross-signed roots and intermediates | Browsers and operating systems |
| [`amazon-n2/`](amazon-n2/) | Amazon N2 roots for service-to-service TLS and mutual TLS (mTLS) certificates issued by AWS Certificate Manager | Only trust stores you configure; not browsers |

Each folder has its own README with the files it contains and how to verify them.

## Files and updates

We provide PEM and DER formatted files for customers to download and install. A file for a root certificate will never change. It is fixed in time. When new roots are added they will be added as new files. If we need to create a new certificate for the same key pair it will be an additional file. When new roots are created they will be added to this repository when they are ready for use.

## Pinning

Amazon Trust Services doesn't recommend or support pinning.

If you require pinning then we recommend that you pin to the public key of the root. Applications that pin to subordinate CAs or end-entity certificates that chain to ATS roots are at a higher risk for outages. We do not recommend pinning to the attributes of a certificate associated with a root. All CAs can have multiple certificates associated with their keys. Because trust is based on keys and not certificate attributes, pinning to certificate attributes may result in unexpected behavior.

## License

The certificate files in this repository are licensed under a [Creative Commons Attribution-NoDerivatives 4.0 International License](https://creativecommons.org/licenses/by-nd/4.0/). The full license text is in [LICENSE](LICENSE).

## Reporting a problem

Report suspected key compromise or other certificate problems at https://www.amazontrust.com/repository/. Do not use GitHub issues for security reports.
