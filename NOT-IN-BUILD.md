# NOT-IN-BUILD: libfal-opennsl

**Status: present for reference. Not built into the image. Not verified. Not supported.**

Hardware-vendor support is out of scope for this project. See the "Hardware support scope" section of the
[`build-iso` README](https://github.com/i-danos/build-iso#hardware-support-scope). This directory is kept in the tree for reference only;
the project makes no commitment to build, test or maintain it. Last commit here: 4fb08f9 2021-11-16.

## What this is

- `libfal-opennsl1`: Broadcom forwarding abstraction layer integration based on OpenNSL

## Measured (image `i-danos_2608_20261030T0752-amd64.hybrid`, sha256 `5d9e25d937fc6dccb8df93bdbdb6a2bc1aa8dd8239491e77d2152f0c0c386fb4`, on 2026-11-02)

| binary package | in the image (SBOM, 1524 pkgs) | named in a package list | built by OBS (repo-index) |
|---|---|---|---|
| `libfal-opennsl1` | no | no | no |

None of these packages is built by this project's OBS repository, and no package list installs them: they are not in the image.

## What provides the corresponding function in the delivered image

No component. The delivered image has no FAL offload backend: forwarding is done by the software data plane (`vyatta-dataplane` on DPDK). The project's earlier observation (the `dpa_object_model_*` rows of `toolkit/release/mk-release.py`, not re-measured on this image) is that `dpa object show` reports each object as held by the software data plane (`sw-dataplane`).

## How to verify this

From the workstation checkout root (`/home/aikon/danos`), with `NAMES="libfal-opennsl1"`:

```
# in the image? (an empty list means no)
python3 -c "import json,sys;n=set(sys.argv[1:]);S=json.load(open('releases/i-danos_2608_20261030T0752-amd64.hybrid/sbom.json'))['packages'];print(sorted(p['name'] for p in S if p['name'] in n))" $NAMES
# named in a package list? (no output means no)
for n in $NAMES; do grep -nE "^[[:space:]]*$n[[:space:]]*(#.*)?\$" build-iso/danos-sources/build-iso/config/package-lists/*.chroot; done
# built by OBS? (1 means yes)
for n in $NAMES; do echo $n $(grep -cE "^Package: $n\$" build-iso/danos-build/repo-index/Packages); done
```
