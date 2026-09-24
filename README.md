# crunchtools packages

Signed dnf and apt repositories for crunchtools command line tools, served
at <https://crunchtools.github.io/packages>. Install instructions are on
that page.

## How it works

`.github/workflows/publish.yml` runs hourly, on demand, and on a
`repository_dispatch` of type `release`. It:

1. downloads the `.rpm` and `.deb` assets from the last five releases of
   every repo listed in `products.txt`;
2. stops if they match the live `index.json` (scheduled runs only);
3. signs every rpm, builds `rpm/` with `createrepo_c` and a signed
   `repomd.xml`, and builds a flat apt repo in `deb/` with a signed
   `InRelease`;
4. deploys the site to GitHub Pages;
5. installs from the live repositories on every target distro, with
   signature checking on, and runs the tool.

No packages are stored in git. The site is rebuilt from the releases every
time, so older versions drop out of the repositories on their own and stay
available on the GitHub releases.

## Adding a tool

Its release workflow attaches a noarch `.rpm` and an `all` `.deb` to each
GitHub release (see crunchtools/petit's `packages.yml`). Add the repo to
`products.txt`.

## Signing key

`crunchtools.asc`, fingerprint
`588C E8BF 2F36 D77E 1B1E 545C C04F 530D 3931 F683`. The private key exists
only as the `PACKAGES_GPG_KEY` Actions secret in this repo.

This repo holds only a workflow and static configuration, so it declares no
crunchtools constitution profile; the tools it publishes carry theirs.
