# cactarium-curate

The curated packages of the [Cactarium](https://github.com/max-morris/Cactarium): the `cactarium.toml`
manifests the Cactarium keeps for repos whose maintainers haven't committed one yet.

A curated package is a stopgap. The Cactarium reads the repo's thorns and release tags as usual, but takes
the package's metadata (name, description, license, contact, how tags become versions) from the manifest
here instead of from the repo. Once a repo carries its own `cactarium.toml`, an admin hands the package
over and its manifest here can be retired.

## Layout

- `curate.toml`: one `[[package]]` per repo: the Cactarium owner (namespace), the repo URL, its manifest,
  and, for repos without usable release tags, versions given by commit.
- `manifests/<name>.toml`: each package's `cactarium.toml`, exactly as the repo would hold it. Each one
  says where its license came from.

Most packages take their versions from the Einstein Toolkit's release tags through the manifest's
`tag-pattern = "ET_{major}_{minor}_v{patch}"` (`ET_2026_05_v0` is version 2026.5.0), from `since` on.

## Applying it

On the Cactarium's host, as an admin:

```bash
just curate <admin> ~/src/cactarium-curate/curate.toml
```

It creates the packages that don't exist yet and updates the ones whose manifest or URL changed; running
it again changes nothing else. A package already handed over to its repo's manifest is left alone.

## Handing a package over

Send the repo's maintainers its `manifests/<name>.toml` to commit as `cactarium.toml` at the repo root
(the name and `owners` must stay as they are). Once it is on the branch their next release is tagged
from, an admin clicks **Use the repo's manifest** for the package in Admin › Curated. The package keeps
its name and versions; from then on each release reads the repo's own file.
