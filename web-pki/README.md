# Web PKI certificates

This folder holds the Amazon Trust Services certificates that browsers and operating systems trust: the roots, the cross-signed roots that link them to older trust stores, and the intermediate CAs that issue TLS certificates. They are the same files published at https://www.amazontrust.com/repository/.

For relying parties that use a custom trust store you will need to add the roots you require. We recommend adding at least Amazon Root CA 1 and Amazon Root CA 4.

## Files

| Path | Contents |
|---|---|
| `roots/pem/`, `roots/der/` | Self-signed roots: Amazon Root CA 1–4 and Starfield Services Root Certificate Authority - G2 |
| `cross-signed-roots/pem/`, `cross-signed-roots/der/` | Amazon Root CA 1–4 cross-signed by Starfield G2 (`G2-RootCA*`, current and `.orig` issuances); Starfield G2 cross-signed by Starfield Class 2 (`SFC2CA-SFSRootCAG2`, two issuances) and by Starfield Services Root Certificate Authority (`SFSRootCA-SFSRootCAG2`) |
| `intermediates/ats-operated/pem/`, `…/der/` | Intermediate CAs operated by Amazon Trust Services |
| `intermediates/externally-operated/pem/`, `…/der/` | Intermediate CAs M01–M04, operated by DigiCert |

## Verify a file

```sh
openssl x509 -in roots/pem/AmazonRootCA1.pem -noout -fingerprint -sha256
```

Compare the result with the fingerprint published at https://www.amazontrust.com/repository/.
