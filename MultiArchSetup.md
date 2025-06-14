# Kpack Multi-Arch Cluster

## 🧰 Tool Intro

* **ko**: Builds Go applications into OCI images.
* **kbld**: Resolves image references, integrates with ko.
* **kapp**: Deploys and manages Kubernetes resources cleanly.

## 🛠️ Build Multi-Arch Images

Use `./hack/local.sh` with ko to build both `arm64` and `amd64` architectures. Run twice:

```bash
./hack/local.sh \
  --build-type ko \
  --registry harbor.yniks.dev/kpack-arm \
  --output kpack-release.yaml \
  --build-args "--platform=linux/arm64"

./hack/local.sh \
  --build-type ko \
  --registry harbor.yniks.dev/kpack-arm \
  --output kpack-release.yaml \
  --build-args "--platform=linux/amd64"

kapp deploy -a kpack -f ./kpack-release.yaml -y
```

## ⚠️ Caveats

* You must run `./hack/local.sh` **once per platform** (e.g. `arm64`, then `amd64`) to create a multi-arch image index.
* ko builds only one platform per invocation.
* You don’t need to touch `.ko.yaml`.
* kbld passes `--platform=...` to ko, but cannot handle comma-separated values.

## ✅ What's Achieved

* Built and published Kpack components (controller, rebase, webhook, etc.) to a private registry as multi-arch images.
* Validated manifests using `docker buildx imagetools inspect`.
* Deployed custom kpack release using `kapp`.

## 🔜 Next Steps

* Define `ClusterStore`, `ClusterStack`, `ClusterBuilder` separately.
* Deploy your apps using `Build` + `Image` CRDs.
* Connect Git repo sources or container sources.
* Optional: use Secrets for private registry access.
