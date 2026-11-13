# stigatron (disabled)

`fleet.yaml` is parked as `fleet.yaml.disabled`, so Fleet does not build a
bundle from this directory. The chart deploys fine -- the images and the
registry override in `values.yaml` are verified -- but both licenses in
`../license/license.yaml` have expired:

| licensee       | notBefore  | notAfter   |
|----------------|------------|------------|
| chris-reynolds | 2023-11-29 | 2024-11-30 |
| philip-brooks  | 2023-02-27 | 2024-02-27 |

Without a current key the compliance and heimdall operators exit immediately:

    STIGATRON is not licensed. Please provide a valid license key. Error: invalid license

That is not something the manifests can work around, and a bundle that can
never go Ready blocks nothing else but does hide real failures, so it is parked
rather than left red.

## Re-enabling

1. Replace `stringData.license` in `../license/license.yaml` with a current key.
2. `git mv fleet.yaml.disabled fleet.yaml`
3. Commit and push; Fleet picks it up on the next poll.

`values.yaml` is already correct and needs no changes: it pins the Carbide
registry plus the three tags Carbide actually mirrors (both operators at 0.4.2,
postgres at 13). If you bump the chart past 0.4.1, re-check those tags first --
the chart's own defaults (0.4.0, 13.1) are not in Carbide.
