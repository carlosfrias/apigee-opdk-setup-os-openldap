# apigee-opdk-setup-os-openldap — OpenLDAP RPM Version-Pinning with Nested Rescue

> **An Ansible role that installs the correct OpenLDAP version for the installed Apigee OPDK release on RHEL/CentOS/OL** — using a four-level nested rescue chain that escalates from a versioned `yum` install all the way to a vendored RPM forced downgrade. The role that makes Apigee LDAP installable across the 4.16/4.17 release matrix on restrictive enterprise mirrors.

Enterprise-Linux packaging engineering expressed as code: the **OpenLDAP version-compatibility matrix** across Apigee releases, the **nested-rescue resilience pattern**, and **library-symlink repair** that this role encodes.

<!-- BEGIN Google Required Disclaimer -->

## Not Google Product Clause

This is not an officially supported Google product.
<!-- END Google Required Disclaimer -->

---

## Why this role is notable

- **A four-level nested rescue chain.** `yum` → versioned downgrade → mirror RPM URLs → `rpm -ivh --oldpackage` from vendored RPMs — escalating force without operator intervention, encoding real-world enterprise-mirror flakiness.
- **Vendoring as last resort.** Ships the exact compatible RPMs in `files/` (`openldap-2.4.40-13.el7` clients + servers) for air-gapped/restrictive networks where every upstream mirror path fails.
- **Library-symlink repair.** Forced RPM downgrades break `liblber`/`libslapi`/`libldap_r` symlinks; the role repairs them because it understands the post-install shared-library state.
- **Version-matrix gating.** Knows exactly which OPDK releases (4.16/4.17) need this pinning and on which OS major (`ansible_distribution_major_version > 6`).
- **Proxy-aware throughout.** Corporate/air-gapped mirrors assumed across every network operation.

---

## What the role actually does

`tasks/main.yml` runs a **four-level nested `block/rescue` chain**, each level a progressively more forceful fallback:

```
Level 1: yum install openldap/openldap-clients/openldap-servers (allow_downgrade=yes)
   └─ rescue:
       Level 2: yum install pinned openldap-2.4.40 (clients + servers)  # version-pin downgrade
          └─ rescue:
              Level 3: yum install openldap_named_versions  # CentOS 6 mirror RPM URLs
                 └─ rescue:
                     Level 4: rpm -ivh --oldpackage {{ openldap_named_versions }}  # forced install from vendored RPMs
```

- **Gated** on `opdk_version <= 4.17.05` and `ansible_distribution_major_version > 6` — the role knows exactly which OPDK releases need this pinning and on which OS.
- **Proxy-aware** — corporate/air-gapped mirrors assumed throughout.
- **Vendored RPMs ship in `files/`** (`openldap-2.4.40-13.el7.x86_64.rpm` — clients + servers) as the last-resort fallback for when every mirror path fails.
- **Library-symlink repair** — `defaults/` declares the dependent library links (`/lib64/liblber-2.4.so.2`, `libslapi-2.4.so.2`, `libldap_r-2.4.so.2`) plus a `libsasl2` support RPM, because a forced OpenLDAP downgrade routinely breaks the shared-library symlinks that the platform expects.

---

## Capabilities — what this credentials

> Ansible is the medium. The engineering below is the evidence of the expertise applied.

- **OpenLDAP version-compatibility matrix** — knowing which OpenLDAP builds are compatible with OPDK 4.16 vs 4.17 and gating the install on the OPDK version + OS major.
- **Nested-rescue resilience pattern** — a four-level fallback (yum → versioned downgrade → mirror URLs → `rpm -ivh --oldpackage`) that escalates force without operator intervention.
- **Vendoring as last resort** — shipping the exact compatible RPMs in `files/` for air-gapped/restrictive networks.
- **Shared-library symlink repair** — forced RPM downgrades break `liblber`/`libslapi`/`libldap_r` symlinks; the role repairs them.
- **Proxy-aware enterprise operations** — `http_proxy`/`https_proxy` threaded through every network operation.

---

---

## When this role is used

Composed into the OPDK OS-prerequisites and LDAP-setup runbooks for OPDK 4.16 / 4.17 deployments. The companion role `apigee-opdk-settings-ldap` handles the LDAP **replication** topology (peer IP, SID, type 1 = single-region vs 2 = multi-master) — this role handles the **packaging** prerequisite that makes the LDAP component installable at all.

See the [`apigee-edge-opdk`](https://github.com/carlosfrias/apigee-edge-opdk) framework and the `apigee-opdk-*` role corpus for the composition playbooks.

## Role variables (selected)

| Variable | Purpose |
|----------|---------|
| `opdk_version` | OPDK release — gates whether version-pinning applies (`<= 4.17.05`) |
| `openldap_named_versions` | The pinned OpenLDAP version string / mirror RPM URLs |
| `ldap_dependent_library_links` | Library symlinks to repair after a forced downgrade (`liblber-2.4.so.2`, `libslapi-2.4.so.2`, `libldap_r-2.4.so.2`) |
| `openldap_support` | Support RPM (e.g. `libsasl2`) required alongside OpenLDAP |

---

## Provenance

Authored and maintained by **Carlos Frias** during his tenure on Apigee Edge Private Cloud. One of the OS/LDAP roles in the `apigee-opdk-*` corpus; the Linux-systems-administration expertise is aggregated in the [`apigee-edge-opdk`](https://github.com/carlosfrias/apigee-edge-opdk) framework.

Contributions welcome — see [CONTRIBUTING.md](./CONTRIBUTING.md).

## License

See [LICENSE](./LICENSE).