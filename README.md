# OpenWrt Feed for batman-adv-next

## Description

The purpose of this repository is to provide the latest batman-adv
release version, plus upcoming features. Plus build compatiblity for
OpenWrt 19.07, 22.03 and 23.05, for Gluon v2021.1.x, v2023.1.x
and v2023.2.x.

## Usage

You can use this in the following Gluon versions:

* Gluon master
* Gluon v2023.2.1 or a later v2023.1.x
* Gluon v2023.1.2 or a later v2023.1.x
* Gluon v2021.1.2 or a later v2021.1.x

For these you can simply add the following to your site/modules file:

```
GLUON_SITE_FEEDS="batman_adv_next"
PACKAGES_BATMAN_ADV_NEXT_REPO="https://github.com/T-X/gluon-batman-adv-next.git"
PACKAGES_BATMAN_ADV_NEXT_COMMIT="<latest-commit>"
PACKAGES_BATMAN_ADV_NEXT_BRANCH="batman-adv-next"
```

Replace `<latest-commit>` to the latest commit you can find
on that branch in this repository.

There is no need for you to update your site.mk. Adding this
repository to your site/modules file is sufficient. batman-adv
is then updated automatically and gluon-mesh-batman-adv-brmldproxy
is pulled in automatically on Gluon master / OpenWrt 23.05.

Also see this for the Gluon site modules concept: https://gluon.readthedocs.io/en/latest/user/site.html#site-modules

For Gluon v2021.1.x additionally make sure you have backported
at least the following two commits:

* "modules: move Gluon packages to front of module list" -> https://github.com/freifunk-gluon/gluon/commit/9e8f58153f664f3d949719b3a5e100cda02000e7
* "scripts/modules.sh: move site feeds to the front of the module list" -> https://github.com/freifunk-gluon/gluon/commit/a20a25e67dbd2e6df02ae704f4be4fd7ff30a9ef

(Or check if they were maybe added to Gluon's v2021.1.x branch
in the meantime.)

## Features

This adds the following features to Gluon:

* Updates batman-adv to v2024.0
  * This adds the new batman-adv multicast packet type:
    * https://patchwork.open-mesh.org/project/b.a.t.m.a.n./cover/20230907010910.22427-1-linus.luessing@c0d3.blue/
      * This allows to transmit multicast to more destinations
        than the so far hard coded limit of 16 in Gluon.
      * Allows to distribute multicast packets more efficiently.
      * See the batman-adv wiki for details: https://www.open-mesh.org/projects/batman-adv/wiki/Multicast-Packet-Type
* Adds a (gluon-mesh-batman-adv-)brmldproxy package on
  Gluon v2023.2 (or later) / OpenWrt 23.05 / Linux 5.15 which allows
  to:
  * inform IPv6 multicast routers of listeners. Allows
    to route IPv6 multicast between domains with a
    daemon like pim6sd: https://github.com/troglobit/pim6sd/
    * Configuration examples: https://dn42.dev/howto/IPv6-Multicast
  * Adds a patch to batman-adv to only send these proxied
    MLD Reports for routable IPv6 multicast addresses
    to IP multicast routers. Instead of flooding these
    reports (default behaviour in batman-adv, with
    these broadcasts being blocked by Gluon by default).

**Notes:**

You will need all nodes in your domain to update to
this batman-adv version for these features to take effect.
Hence the batman-adv version provided here was patched to also
work on Gluon v2021.1.x. You can check the output of
`batctl mcast_flags` for instance to verify that all nodes
are running with these new features enabled.

IPv6 multicast listeners for routable addresses with
multicast router support are only supported on nodes
running Gluon v2023.2 (or later) / OpenWrt 23.05. So if you
want to receive a multicast stream from another
broadcast domain via an IPv6 multicast router then
your listener needs to be connected to a Gluon v2023.2 (or later)
/ OpenWrt 23.05 based node (with the updated packages
from here). (However if the multicast sender is in the same
broadcast domain, with no need for a multicast router,
you can then also receive it with a multicast listener
on Gluon v2021.1.x or v2023.1.x)
