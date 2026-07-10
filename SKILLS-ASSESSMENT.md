# Skills Assessment — apigee-opdk-setup-os-openldap

> **Skill domain:** Enterprise-Linux packaging engineering and resilience-pattern design — Apigee Edge Private Cloud (OPDK). Part of the broader Apigee platform-operations portfolio; see the [`bap_coe` portfolio hub →](https://github.com/carlosfrias/apigee-hybrid-workspace/blob/master/SKILLS-ASSESSMENT.md) for the cloud-native (Hybrid/K8s) counterpart and the full corpus.

---

## Why this role is notable

- **A four-level nested rescue chain.** `yum` → versioned downgrade → mirror RPM URLs → `rpm -ivh --oldpackage` from vendored RPMs — escalating force without operator intervention, encoding real-world enterprise-mirror flakiness.
- **Vendoring as last resort.** Ships the exact compatible RPMs in `files/` (`openldap-2.4.40-13.el7` clients + servers) for air-gapped/restrictive networks where every upstream mirror path fails.
- **Library-symlink repair.** Forced RPM downgrades break `liblber`/`libslapi`/`libldap_r` symlinks; the role repairs them because it understands the post-install shared-library state.
- **Version-matrix gating.** Knows exactly which OPDK releases (4.16/4.17) need this pinning and on which OS major (`ansible_distribution_major_version > 6`).
- **Proxy-aware throughout.** Corporate/air-gapped mirrors assumed across every network operation.

---

## Expertise demonstrated

> Ansible is the medium. The engineering evidence lives in the [project README →](README.md). What follows is the skills assessment for the business reader.

- **OpenLDAP version-compatibility matrix** — knowing which OpenLDAP builds are compatible with OPDK 4.16 vs 4.17 and gating the install on the OPDK version + OS major. This is the domain knowledge, not the automation.
- **Nested-rescue resilience pattern** — a four-level fallback (yum → versioned downgrade → mirror URLs → `rpm -ivh --oldpackage`) that escalates force without operator intervention. The pattern is the engineering — encoding real-world enterprise-mirror flakiness as escalating resilience.
- **Vendoring as last resort** — shipping the exact compatible RPMs in `files/` for air-gapped/restrictive networks. The judgment of *when* to vendor (last level, not first).
- **Shared-library symlink repair** — forced RPM downgrades break `liblber`/`libslapi`/`libldap_r` symlinks; the role repairs them because it understands the post-install shared-library state.
- **Proxy-aware enterprise operations** — `http_proxy`/`https_proxy` threaded through every network operation.

---

## How this shows the expertise

This role is enterprise-Linux packaging engineering expressed as code. The expertise is not "installing OpenLDAP" — it is the **nested-rescue resilience pattern**: a four-level fallback that escalates force (`yum` → versioned downgrade → mirror URLs → vendored-RPM forced install) without operator intervention, encoding real-world enterprise-mirror flakiness.

The clearest single signals:
1. **The four-level rescue chain** — a resilience design that degrades gracefully through progressively more forceful fallbacks. That is failure-mode engineering, with Ansible as the medium.
2. **Library-symlink repair** — knowing that a forced RPM downgrade breaks `liblber`/`libslapi`/`libldap_r` symlinks, and repairing them because the post-install shared-library state is understood. That is Linux systems administration, not "an Ansible role."

---

## Related expertise

| Skill | Repository | Assessment |
|-------|-----------|-----------|
| SAML SSO / dual-keypair (companion) | [`apigee-opdk-setup-edge-sso-config`](https://github.com/carlosfrias/apigee-opdk-setup-edge-sso-config) | [SKILLS-ASSESSMENT.md →](https://github.com/carlosfrias/apigee-opdk-setup-edge-sso-config/blob/master/SKILLS-ASSESSMENT.md) ✅ |
| OPDK framework (flagship monorepo) | [`apigee-edge-opdk`](https://github.com/carlosfrias/apigee-edge-opdk) | [SKILLS-ASSESSMENT.md →](https://github.com/carlosfrias/apigee-edge-opdk/blob/master/SKILLS-ASSESSMENT.md) *(pending retrofit)* |
| Cloud-native (Hybrid/K8s) counterpart | [`apigee-hybrid-workspace`](https://github.com/carlosfrias/apigee-hybrid-workspace) | [SKILLS-ASSESSMENT.md →](https://github.com/carlosfrias/apigee-hybrid-workspace/blob/master/SKILLS-ASSESSMENT.md) ✅ portfolio hub |

---

## Provenance

Authored and maintained by **Carlos Frias** during his tenure on Apigee Edge Private Cloud. This skills assessment is the companion to the engineering [README →](README.md). For the full engineering detail — the rescue chain, variables, and composition — see the project README.

## License

See [LICENSE](./LICENSE).