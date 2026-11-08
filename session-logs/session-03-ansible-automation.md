# Session 03: Automated Baseline Hardening & Private Registry Distribution

- **Date:** November 2026
- **Objective:** Deploy and verify Ansible control plane on `core-services.lab` (192.168.56.10), enforce DISA STIG-aligned baselines, and automate offline Docker runtime deployment with insecure registry trust across enclave targets.

## Milestones Achieved
1. **Offline Ansible Control Engine Bootstrap:**
   - Satisfied `ansible-core` (2.14.x) dependencies under Python 3.9.21 via staged offline RPMs (`python3-cryptography`, `python3-packaging`, `python3-resolvelib`).
   - Resolved callback plugin initialization by enforcing `stdout_callback = default` in `ansible.cfg`.
2. **Keypair Governance & Inventory:**
   - Established passwordless SSH trust from `core-services.lab` to `siem.lab` (192.168.56.20) and `k8s-node.lab` (192.168.56.30).
   - Structured hierarchical inventory in `ansible/inventories/hosts.ini` with target children groups.
3. **Automated Hardening (`01-baseline-harden.yml`):**
   - Deployed STIG-compliant login banners in `/etc/motd`.
   - Ensured Chrony time-sync service is enabled and started.
   - Enforced `0000` root permissions on `/etc/shadow`.
   - Verified 100% idempotency (`ok=4, changed=0, failed=0` on re-run).
4. **Container Engine & Registry Integration (`02-docker-install.yml`):**
   - Automated offline Docker Engine RPM installation across all target nodes.
   - Configured `/etc/docker/daemon.json` with insecure registry trust pointing to `192.168.56.10:8082`.
   - Handled Docker Bearer Token Realm authentication.
   - Verified authenticated container image distribution by pulling `192.168.56.10:8082/nexus3:v1` directly onto target nodes (`siem.lab` and `k8s-node.lab`).

## Enclave State & Next Action
- **Current State:** Registry pushes/pulls verified; all nodes running Docker and hardened.
- **Next Session Backlog:** Provision Elasticsearch 8.x and Kibana containers onto `siem.lab` (`vm.max_map_count=262144` applied) and configure audit log forwarding.
