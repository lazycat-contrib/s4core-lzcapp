# S4 LazyCat App

This directory contains an LPK v2 conversion for S4.

Upstream repository: https://github.com/s4core/s4core
Official site: https://s4core.com
Upstream release used for package metadata: v1.0.0-beta-federation

## Entrypoints

- Console: default app domain `/` and listed first as the default launcher entry.
- S3 API: `api-<app-domain>/` via `domain_prefix: api`, so S3 client request paths and signatures are not shifted by a URL prefix.

`public_path` includes `/` because S3 clients cannot use LazyCat browser authentication. This also makes the console reachable without LazyCat auth, so the console relies on S4 IAM credentials.

## Image Handling

Images are intentionally left as source-style image names:

- `s4core/s4core:latest`
- `s4core/s4console:latest`

Replace them in `lzc-manifest.yml` with your LazyCat registry images before release. The source compose used a local build for `s4core`, so this package does not try to build or copy images automatically.

## Generated And Configurable Values

Deployment parameters expose the Root account, S3 access keys, upload limit, lifecycle worker, compaction worker, metrics, and S3 Select settings.

`S4_JWT_SECRET` is generated with `stable_secret`, so it remains stable for the installed app without being shown in the setup wizard.

`S4_MODE` is fixed to `single` because this LPK packages a single S4 node. Federation variables are intentionally not exposed in the setup wizard.

## Build

```bash
lzc-cli project release -o s4.lpk
```

The app includes the LazyCat file chooser browser inject for console upload/download flows.
