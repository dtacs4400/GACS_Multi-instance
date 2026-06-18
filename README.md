# 🚜 AGNIX_GACS_Multi-instance — GenieACS Multi-Instance

CLI manager to deploy, monitor, and manage **multi-instance GenieACS (TR-069 ACS)** on a single VPS, with L2TP VPN integration for local ONU connectivity.

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Platform](https://img.shields.io/badge/platform-Ubuntu%2022.04-orange.svg)
![GenieACS](https://img.shields.io/badge/GenieACS-v1.2%20%7C%20v1.3-green.svg)

---

## ✨ Features

| Features | Description |
|---|---|
| **Multi-Instance** | Deploy multiple GenieACS instances on a single server, each isolated |
| **Auto Port Allocation** | CWMP/NBI/FS/UI ports are automatically allocated without conflicts |
| **Nginx Reverse Proxy** | Automatic subdomain per instance (`acs-<name>.domain.id`) |
| **Wildcard SSL/HTTPS** | SSL via Let's Encrypt + Cloudflare DNS-01 challenge |
| **L2TP VPN Integration** | Automatically create L2TP users per instance for MikroTik connections |
| **ONU Route Management** | Automate ONU subnet routing so ACS can summon/push devices |
| **Parameter Restore** | Restore provisions, virtual parameters, presets, and UI configuration from presets |
| **Version Support** | GenieACS Stable (v1.2) and Latest (v1.3-dev) |
Pause/Unpause | Freeze instances without stopping containers |
Activity Log | History of all actions with filters and search |

---

## 🏗️ Architecture

```
┌────────────────────────────────────────────────────────────────────────┐
│ VPS (Cloud) │
│ │
│ ┌──────────┐ ┌──────────┐ ┌──────────┐ │
│ │ Instance1 │ │ Instance2 │ │ Instance3 │ ... │
│ │ GenieACS │ │ GenieACS │ │ GenieACS │ │
│ │ +MongoDB │ │ +MongoDB │ │ +MongoDB │ │
│ └────┬─────┘ └────┬─────┘ └────┬─────┘ │
│ │ │ │ │
│ ┌────┴────────────── ┴──────────────┴────┐ │
│ │ Nginx Reverse Proxy │ │
│ │ (SSL/HTTPS + Subdomains) │ │
│ └───────────────────────────────────────┘ │
│ │ │
│ ┌─────────────────── ─┴──────────────────┐ │
│ │ L2TP VPN Server │ │
│ │ 172.16.101.1 (Gateway Server) │ │
│ └────────┬──────────┬──────────────────┘ │
│ │ │ │
└───────────┼───────────┼───── ──────────────────────────────┘ 
│ │ 
L2TP Tunnel L2TP Tunnel 
│ │ 
┌────────┴──┐ ┌─────┴──────┐ 
│ MikroTik1 │ │ MikroTik2 │ 
│172.16.101.│ │172.16.101. │ 
│ 10 │ │ 11 │ 
└─────┬─────┘ └─────┬─────┘ 
│ │ 
┌────┴────┐ ┌────┴────┐ 
│ONU/ONT │ │ONU/ONT │ 
│10.50.x.x│ │192.168.x│
└─────────┘ └─────────┘
```

---

## 📦 Installation

### Prerequisites
- **OS**: Ubuntu 22.04+ (or Debian-based)
- **Docker & Docker Compose**: [Install Docker](https://docs.docker.com/engine/install/ubuntu/)
- **Git & Curl**: `apt install git curl`
- **Domain** with Wildcard DNS Record (`*.domain.id → VPS IP`)
- **Cloudflare API Token** (for SSL)

> The script automatically checks root permissions and all dependencies when run.

### Quick Start

```bash
# 1. Clone repository (free path)
git clone https://github.com/dtacs4400/GACS_Multi-instance.git /home/docker/genieacs
cd /home/docker/genieacs/manager

# 2. Run the manager (must be root)
chmod +x agnix_gacs.sh
sudo ./agnix_gacs.sh
```

> Free path clone, auto-detect location script. Examples: `/opt/genieacs`, `/root/gacs`, etc.

### 🚀 First-Time Setup (Fresh VPS)

Follow this sequence in the manager:

```
┌─ 3. Services & Settings
│ ├─ 4. Setup GenieACS Source ← Clone source stable/latest
│ ├─ 2. Install Services ← Install L2TP, Nginx, Certbot
│ └─ 1. Setup Domain & SSL ← Configure domain + SSL
│
└─ 1. Manage Instance
└─ 1. Install New Instance ← Deploy first GenieACS
```

**Detailed steps:**
1. Select `3` → Services & Settings → `4` Setup GenieACS Source → Clone Stable/Latest
2. Select `3` → Services & Settings → `2` Install Services → Install All
3. Select `3` → Services & Settings → `1` Setup Domain & SSL
4. Select `1` → Manage Instance → `1` Install New Instance

---

## 🎮 Usage

```bash
cd /home/docker/genieacs/manager
sudo ./agnix_gacs.sh
```

### Play Menu
```
╔═════════════════════ ═════════════════════╗
║ AGNIX GACS MANAGER v1.2 ║
║ GenieACS Multi-Instance Orchestrator ║
╚═════════════════════ ═════════════════════╝ 

Instances: 1 │ Domain: domain.id │ SSL: Active │ L2TP: Active │ Docker: Active
───────────────────── ───────────────────── 
1. Manage Instances 
2. View Activity Log 
3. Services & Settings 
0. Exit
```

### [1] Manage Instance
``` 
1. Install New Instance 
2. Monitor Resources 
3. Pause /
