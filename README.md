# nib-site

Public website for Nib (iOS): privacy policy and support pages, served by GitHub Pages.

https://keithxc.github.io/nib-site/

## Official recognition rules (contract v1)

Nib applies these declarative suggestions on the phone. The single fixed public
feed is `https://keithxc.github.io/nib-site/rules/v1/manifest.json`; its `version`
selects `pack-<version>.json` in the same directory. No custom feed URL, scripts,
regular expressions, receipt samples, or private data belong in this feed.

The manifest contains only `schemaVersion: 1`, a positive integer `version`, and
`sha256` (64 lowercase hexadecimal characters). The digest covers the **exact
downloaded pack bytes**, including whitespace and the final newline. The pack
contains `schemaVersion: 1`, the same `version`, and `languages` with all five keys:
`zh-Hans`, `zh-Hant`, `ja`, `ko`, and `en`. Each language contains:

- `categories`: built-in category IDs mapped to arrays of literal keywords.
  The only IDs are `food`, `transport`, `shopping`, `home`, `health`, `digital`,
  `investment`, `salary`, `benefit`, and `other`.
- `noteLabels`: exact unhelpful status, UI, or field labels to demote as notes.
- `preferredLabels`: merchant or product field labels that favor the original
  value to the right, after a colon, or on the following line, rather than the
  label itself. Receipt note text stays original; labels do not translate or
  rewrite it.

All languages are active together, including mixed-language travel receipts;
interface language, device region, and currency never select or gate rules.
The first pack covers dining (`food`), groceries and mixed-goods convenience
stores (`shopping`), transport, and other clear everyday cues. Lodging uses
`home`. Built-in categories reuse stable IDs. `investment` is an expense
category, so its keywords cover only clear purchases or fees; confirmation uses
the category’s income/expense flag. Ambiguous payment words, bare currency
symbols, and broad merchant fragments are excluded from category keywords.
`other` intentionally has no keywords. Suggestions can be corrected by the user
and do not migrate or overwrite existing entries, categories, notes, or custom
rules.

## Publishing and compatibility

1. For each update, create a **new**, strictly increasing integer version and
   `rules/v1/pack-<version>.json`. Keep `schemaVersion` at 1 for compatible data
   changes. Retain all five languages and stable IDs; review every locale for
   ambiguous matches, duplicate/conflicting keywords, and label quality.
2. Validate JSON and the contract, then finalize UTF-8 bytes (no BOM). Compute
   `shasum -a 256 rules/v1/pack-<version>.json`. Copy that exact lowercase digest
   into a manifest with the matching version. Do not reformat the pack afterward.
3. Publish the new pack **before** advancing the manifest. Fetch the published
   pack URL, verify the downloaded bytes against the digest, then publish the
   manifest. Re-fetch both after deployment and verify their versions and hash.
   Never edit, replace, or delete a published pack; a correction gets a new
   version. To withdraw bad rules, republish known-good content under a higher
   version instead of decreasing the manifest version.
4. Compatible rule releases need only the feed update; an app release is not
   required. Refresh the bundled `Resources/official-rules-v1.json` snapshot with
   validated pack bytes in a later app release. App compatibility and bundle
   changes are reviewed in the private app project; this repository has no app
   code. Breaking contracts require a new schema/path and compatible app support;
   do not publish them to v1.

In compatible app versions, optional official-rule downloads are enabled by
default and can be turned off in Settings. After a successful check, the next
check is due in 72 hours; a failed check can retry after one hour while the app
is in use. Entering the foreground or opening Settings can trigger a due check.
Clients validate the manifest, version, digest, and pack contract before adopting
an update. Offline,
failed, corrupt, or incompatible updates retain the last valid local pack or
bundled fallback. Recognition and suggestions remain local. Requests use fixed
static file URLs and carry no receipt/ledger content or identifiers added by Nib;
GitHub Pages may process ordinary request metadata such as IP addresses.
