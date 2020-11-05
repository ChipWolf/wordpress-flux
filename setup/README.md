# installation (one-time actions)

```shell
./bootstrap-cluster.sh
```

The [script](bootstrap-cluster.sh) does the following:

1. Installs [flux](https://github.com/fluxcd/flux)
2. Retrieves the new flux public key and saves it to the GitHub repo as a deploy key _(see [add-repo-key.sh](add-repo-key.sh))_
