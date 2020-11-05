# sealed-secrets

[Sealed Secrets](https://github.com/bitnami-labs/sealed-secrets) deployment

**Problem:** "I can manage all my K8s config in git, except Secrets."

**Solution:** Encrypt your Secret into a SealedSecret, which *is* safe
to store - even to a public repository.  The SealedSecret can be
decrypted only by the controller running in the target cluster and
nobody else (not even the original author) is able to obtain the
original Secret from the SealedSecret.
