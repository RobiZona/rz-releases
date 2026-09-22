# RZ Development — releases

[versions.json](versions.json) contains everything, grouped by script:

- `version`: latest available release.
- `minimum_version`: oldest version that may run; lower versions must update.
- `changelog`: changes grouped by version, newest first. An empty `[]` means no notes.

The console and in-game menu show changes newer than the installed version.
Download updates through your Tebex/Cfx purchase, replace the script folder,
and keep your database.

## Product channels and legacy migration

Every new release declares `rz_update_product` in its manifest. That value is
the key used in this file, independently of the installed resource folder.
The SDK build embeds it as `update_product_id`; its updater, cache and release
workflow use that same field. The SDK's runtime `product_id` remains unchanged
for resource naming, storage, permissions and menu coordination.

| Product edition | Installed resource name | `rz_update_product` / registry key |
| --- | --- | --- |
| Herbs V1 / free | `rz_herbs` | `rz_herbs_free` |
| Herbs V2 | `rz_herbs` | `rz_herbs_v2` |
| Eggs V1 / free | `rz_eggs` | `rz_eggs_free` |
| Eggs V2 (reserved) | `rz_eggs` | `rz_eggs_v2` |

The old `rz_herbs` entry in this registry is a compatibility feed for previous
**V2 SDK builds**, which used their runtime product name as the update key.
It advertises the V2 transition release. Keep its history and entry so those
builds can find the migration. Subsequent ordinary V2 releases are written to
`rz_herbs_v2`; the workflow does not overwrite the compatibility feed.
This entry is separate from the **V1 legacy GitHub repository** described below.

Free migration releases: **Herbs free 1.0.8** and **Eggs free 1.0.9**.
Publish the same release version and changes in both locations:

- Legacy `RobiZona/rz_herbs/docs/changelog.json` and `RobiZona/rz_eggs/docs/changelog.json`: old free installations see the migration release through their existing checker.
- This repository's `versions.json`, under `rz_herbs_free` / `rz_eggs_free`: updated free installations use their explicit edition channel.

Keep legacy history. Do not put paid V2 version numbers in either legacy free
feed: old checkers pick the highest version and cannot distinguish editions.
The prepared legacy files live in the corresponding free resource's
`public/docs/changelog.json`; publish their content at `docs/changelog.json`
in the old public repository.

Make complete resource packages downloadable before publishing announcements.
Customers must install the complete updated resource and merge their public
customizations; replacing only `public/` does not install the new checker.
Free standalone adapters provide console notifications; the SDK's minimum-version
gameplay gate remains specific to SDK-based products. The update ID rule is the
same for both.
