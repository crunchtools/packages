# packages Constitution

> **Version:** 1.0.0
> **Ratified:** 2026-10-02
> **Status:** Active
> **Inherits:** [crunchtools/constitution](https://github.com/crunchtools/constitution) v1.18.0
> **Profile:** Package Repository

This file holds what is specific to crunchtools/packages. The fleet rules and
the Package Repository profile apply at the inherited version and are checked
against this repo's files by `constitution.yml`. They are not restated here.

## Products and Formats

- **Products:** the repos listed in `products.txt` (today
  `crunchtools/petit`). Each product's release workflow attaches a noarch
  `.rpm` and an `all` `.deb` to its GitHub releases.
- **Retention:** the last five non-draft, non-prerelease releases of each
  product (`KEEP_RELEASES`). Older versions drop out of the repositories and
  stay on the GitHub releases.
- **Formats:** a dnf repository in `rpm/` (signed packages, signed
  `repomd.xml`, `repo_gpgcheck=1`) and a flat apt repository in `deb/`
  (signed `InRelease` and `Release.gpg`), served from GitHub Pages at
  <https://crunchtools.github.io/packages>.
- **Signing key:** `crunchtools.asc`, fingerprint
  `588C E8BF 2F36 D77E 1B1E 545C C04F 530D 3931 F683`. The private key is the
  `PACKAGES_GPG_KEY` Actions secret; the workflow refuses to sign if the
  secret's fingerprint differs from the committed public key.

## Install Smoke Test

After every deploy, `publish.yml` installs each product from the live
repositories with signature checking on and runs it, on UBI 8, 9 and 10,
AlmaLinux 9, CentOS Stream 10, Fedora, Amazon Linux 2023, SUSE BCI 15.7 and
16.0, Debian 12 and 13, and Ubuntu 24.04 and 26.04. Downloads retry in a
loop because Pages returns 503s right after a deploy and UBI 8's curl lacks
the newer retry flags.

## Hourly Schedule

`publish.yml` keeps an hourly `schedule:` trigger alongside
`repository_dispatch` and `workflow_dispatch`. A scheduled run with nothing new
stops after comparing the release assets against the live `index.json`, and
each run re-enables the workflow so GitHub's 60-day idle rule cannot switch
the schedule off.

## History

| Version | Date | Changes |
|---------|------|---------|
| 1.0.0 | 2026-10-02 | Initial manifest under constitution v1.18.0 |
