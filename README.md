# Quantum Bridge Code Signing Attestations

## Overview

This repository supports "keyless" code signing using the blockchain based code signing service, sigstore.

Our goal is to sign the binaries we distribute to customers so that customers can verify that their integrity. We'd like to minimize the number of third parties the customer has to trust other than Quantum Bridge.

Cloudsmith has infrastructure to sign packages with a gpg key, which is used for signing the index and packages for RPMs, but this key is controlled by Cloudsmith, so while the customer can validate that there wasn't a man-in-the-middle attack between Cloudsmith and the customer, it requires placing absolute trust in Cloudsmith to not be tampering with packages.

For additional security, we publish sha256 checksums of our packages, and publish attestations about their validity to sigstore. Sigstore provides a tamper-proof public ledger of attestations that the customer can use to verify that a given binary came from Quantum Bridge.

This code signing process is currently not post-quantum safe, something to investigate in the future.

## Customer Validation

To verify a given artifact, the customer will download the binary at a given VERSION and the corresponding checksums.txt.

### Usage

The user will verify the checksums.txt:

```
gh attestation verify checksums.txt -R quantumbridgetech/qbt-attestations
```

They will then verify the artifacts match the checksums:

```
sha256sum --ignore-missing -c checksums.txt
```

Only is the above two steps succeed will they proceed with the install.
