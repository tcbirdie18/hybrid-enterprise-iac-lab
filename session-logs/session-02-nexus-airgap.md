# Session 02: Core Services & Air-Gapped Registry Bootstrap

- **Objective:** Provision `core-services.lab` (192.168.56.10) and deploy Sonatype Nexus OSS under zero-WAN egress.
- **Air-Gap Verification:** 
  - Dropped default gateway route on `enp0s3` (`curl: (7) Couldn't connect to server`).
  - Purged online repository configs and executed local installation via `rpm -ivh`.
- **Deployed Services:**
  - Docker Engine 27.x + Compose plugin installed from local RPMs.
  - Sonatype Nexus OSS 3.x imported via offline `nexus3.tar` archive.
  - Persistent volume configured at `/opt/nexus/data` (chown 200:200).
  - Private Docker hosted registry exposed on HTTP port 8082.
  - Active Realm: Docker Bearer Token Realm.
- **Validation:** 
  - Authenticated via `docker login 192.168.56.10:8082`.
  - Pushed initial test image `192.168.56.10:8082/nexus3:v1` and verified asset storage via Nexus GUI.
