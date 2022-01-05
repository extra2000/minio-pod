# Changelog

## [2.2.0](https://github.com/extra2000/minio-pod/compare/v2.1.0...v2.2.0) (2022-01-05)


### Features

* **dockerfile:** update MinIO from `RELEASE.2021-12-10T23-03-39Z.fips` to `RELEASE.2022-01-04T07-41-07Z.fips` ([f786e9e](https://github.com/extra2000/minio-pod/commit/f786e9eef5ceae3f299405a32217a19af896cc70))

## [2.1.0](https://github.com/extra2000/minio-pod/compare/v2.0.0...v2.1.0) (2021-12-14)


### Features

* **pods:** add mountpoints for SSL certificate ([ebd2fae](https://github.com/extra2000/minio-pod/commit/ebd2fae30e8b52772c84fc4351387f3be0c99c7a))
* **pods:** add SSL certs files ([6c3fa79](https://github.com/extra2000/minio-pod/commit/6c3fa793306d15308655d4d019944c015cefd8b0))


### Documentations

* **deployments:** add instructions to create SSL certs ([95d6c62](https://github.com/extra2000/minio-pod/commit/95d6c621506e9068f1a8cd22364f2b5e802e83bb))
* **deployments:** change `http://127.0.0.1:9001` to `https://127.0.0.1:9001` ([b010cac](https://github.com/extra2000/minio-pod/commit/b010cac8f74d7e53885f18217b22f17bb8810569))


### Code Refactoring

* **podman-general:** using `hostNetwork` because HTTPS doesn't work with NAT ([d6710f7](https://github.com/extra2000/minio-pod/commit/d6710f775f9c61cc1e5482adee13a3c4ad56f0f3))
* **secrets:** remove `.gitkeep` ([d651677](https://github.com/extra2000/minio-pod/commit/d6516778de852139214d363954d66f6a1ac2d41d))


### Continuous Integrations

* **AppVeyor:** remove `podman play kube` instructions ([a79dd53](https://github.com/extra2000/minio-pod/commit/a79dd53ac7e13744bf9b40a0dacba383be36a8bd))

## [2.0.0](https://github.com/extra2000/minio-pod/compare/v1.0.0...v2.0.0) (2021-12-14)


### ⚠ BREAKING CHANGES

* **deployments:** we no longer support rootful Podman deployments
* **pods:** pod files have been changed into example files

### Features

* **minio:** upgrade from `RELEASE.2021-08-31T05-46-54Z.fips` to `RELEASE.2021-12-10T23-03-39Z.fips` ([357cd04](https://github.com/extra2000/minio-pod/commit/357cd04ad5b0f6fbabfe6ffc4d5c473a9aebc253))


### Code Refactoring

* **pods:** make pod files as example files ([003ce27](https://github.com/extra2000/minio-pod/commit/003ce275bdd3e9829ddba40333e71d7608b67c28))
* **selinux:** remove unnecessary `user_home_t` ([7dbba7e](https://github.com/extra2000/minio-pod/commit/7dbba7e71153e65dd95a2e46d8290162ccd1afbe))


### Styles

* **sphinx:** change to full width page ([d2a6559](https://github.com/extra2000/minio-pod/commit/d2a65599976ca0d06cbc7f3549a2bc1144cb58ac))


### Documentations

* **deployments:** add instructions to create pod file ([52f2693](https://github.com/extra2000/minio-pod/commit/52f269356b285f61a410072c03c95b6c7cb2e8b4))
* **deployments:** change from rootful to rootless Podman deployment ([a449e95](https://github.com/extra2000/minio-pod/commit/a449e95e18c0a86058f1b4ae9b90a7dc36d4c43e))
* **deployments:** improve `cd` instructions ([7aa7893](https://github.com/extra2000/minio-pod/commit/7aa7893e62e0f48f3687971552c2d9d8b337e48a))
* **host-preparations:** update instructions for AlmaLinux 8.5 and above ([860bf9e](https://github.com/extra2000/minio-pod/commit/860bf9ea33031cb31e4784ec4744e1e63fdb03e4))


### Continuous Integrations

* **AppVeyor:** add instruction to create pod file ([8dfdac9](https://github.com/extra2000/minio-pod/commit/8dfdac9e0b9e10350781e3f8c58e8cce3b258675))
* **AppVeyor:** change from Ubuntu `18.04` to Ubuntu `20.04` ([8f356c6](https://github.com/extra2000/minio-pod/commit/8f356c607d4d5b301aab8c34f6b609dd72220905))
* **AppVeyor:** ensure CA certs are latest ([c69e6aa](https://github.com/extra2000/minio-pod/commit/c69e6aa3d3cf00f3224dc1a2115be4b8d59b8eaa))

## 1.0.0 (2021-09-05)


### Features

* initial commit ([deeed2c](https://github.com/extra2000/minio-pod/commit/deeed2c3c7947138042d7e5164ee7053c4384541))


### Documentations

* **README:** update `README.md` ([af21e37](https://github.com/extra2000/minio-pod/commit/af21e3773bc7b81b0c4ea0165a8958a6fb1e4eb3))


### Continuous Integrations

* add AppVeyor with `semantic-release` ([a1b1024](https://github.com/extra2000/minio-pod/commit/a1b102434ddc591118017b76604a088b75bc9399))
