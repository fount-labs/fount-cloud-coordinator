# fount-cloud-coordinator

The Tinfoil config for Fount Cloud's coordinator enclave. Each release here is measured and
attested by Tinfoil. Fount's runner enclaves take jobs only over a link from an enclave whose
attestation names a release of this repository, and Fount's inference proxy trusts a job's
credential only if it is signed by a key such an enclave attested.

**Test data only, for now.** It takes jobs through a test API behind a bearer token, and its
wrapping key is made at boot rather than held by Fount's key vaults. Both are replaced before it
holds anyone's data (Fount m4-cloud-runners.md §7, P2).

The enclave runs one image, built from [fount-broker](https://github.com/fount-labs/fount-broker)
by its release workflow and pinned by digest in `tinfoil-config.yml`:

- **coordinator**: `fount-coordinator`. It queues jobs and sends each to a runner enclave built
  from [fount-cloud-runner](https://github.com/fount-labs/fount-cloud-runner), over a link both
  ends attest. It signs each job's credential with its attested `job-signing` key.

To release: pin the digest on `main`, then an org admin pushes a `v*` tag; **Tinfoil Release**
measures, attests and publishes it, and marks it latest so Tinfoil witnesses it. Only org
admins can create `v*` tags, and none can be moved or deleted.
