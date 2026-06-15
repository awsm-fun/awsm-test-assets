# awsm-test-assets

Test/fixture assets for [awsm-renderer](https://github.com/awsm-fun/awsm-renderer)
(scene editor + model tester).

## Deploy

Assets are served from the `awsm-cdn` R2 bucket under the `test-assets/` prefix
at **https://cdn.awsm.fun/test-assets/**. Deployment is a manual `task` (no CI):

```sh
cp .env.example .env   # fill in the Cloudflare R2 + purge credentials
task deploy            # rclone sync → cdn.awsm.fun/test-assets/
task purge             # (optional) invalidate the edge cache immediately
```

`task deploy` mirrors the repo to R2; what ships is controlled by
[`cdn-filter.txt`](./cdn-filter.txt) (large source files — `.exr`/`.blend` and
the `photo_studio/skybox`, `ibl-*` source dirs — are excluded; the derived
runtime assets are uploaded). Add a new asset by committing it; remove one from
the CDN by deleting it or excluding it in the filter.

Other tasks: `task ls` (list the prefix in R2), `task check [-- <path>]` (HEAD an
asset, show cache/CORS headers).

## Live URLs

* https://cdn.awsm.fun/test-assets/photo_studio/skybox.ktx2
* https://cdn.awsm.fun/test-assets/photo_studio/env.ktx2
* https://cdn.awsm.fun/test-assets/photo_studio/irradiance.ktx2
* https://cdn.awsm.fun/test-assets/gizmo/gizmo.glb

> CORS: the bucket must allow the consuming origins (`scene.awsm.fun`,
> `model-tests.awsm.fun`) for cross-origin fetches — configured on the bucket in
> the Cloudflare dashboard.
