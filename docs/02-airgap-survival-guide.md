# Air-Gap Architecture & Ingestion Survival Guide

In disconnected, classified, or zero-trust enclaves, all external internet egress is blocked at the routing layer.

## 1. Core Services Hub (192.168.56.10)
- **Nexus OSS 3.x:** Functions as the authoritative image and artifact registry.
- **Docker Port 8082:** Dedicated HTTP connector for Docker image pushes and pulls.
- **Anonymous Access:** Enabled for internal read pulls; write actions require authenticated credentials.

## 2. Ingestion Pattern
1. Download upstream archives, packages, or images to host staging drop: `C:\lab\hybrid-enterprise-iac-lab\transfer_drop\`.
2. Sync/mount drop directory inside target nodes at `/opt/transfer_drop/`.
3. Offline packages installed via local RPM bundle: `rpm -ivh /opt/transfer_drop/docker-rpms/*.rpm`.
4. Import raw container archives into daemon storage: `docker load -i /opt/transfer_drop/images/*.tar`.
5. Push validated container images directly to `192.168.56.10:8082`.
