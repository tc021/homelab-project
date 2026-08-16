# Homelab Infrastructure

A self-hosted laboratory environment designed for learning and experimenting
with networking, Linux administration, databases, automation, containerization,
machine learning, and infrastructure management.

The project is built from scratch, starting with virtualization and network
configuration and gradually evolving into a multi-service homelab environment.

The main goal is not only to deploy working services, but also to understand,
configure, document, test, and automate the infrastructure behind them.

---

## Project Goals

The project provides a practical environment for developing skills in:

- Linux system administration
- Computer networking
- Virtualization
- Infrastructure design
- Git and collaborative development workflows
- Containerization
- Database administration
- Infrastructure automation
- Monitoring and observability
- Security and network segmentation
- Machine learning infrastructure
- Game server hosting

The environment is intentionally developed incrementally. Each major component
is configured manually first to understand the underlying technology before
introducing automation.

---

## Architecture

The current laboratory environment is based on Oracle VirtualBox running on
a Windows host.

The initial Ubuntu virtual machine uses two virtual network interfaces:

```text
                         Internet
                            |
                            |
                      VirtualBox NAT
                            |
                         enp0s3
                       10.0.2.15
                            |
                    +---------------+
                    |               |
                    | Ubuntu DEV01  |
                    |               |
                    +---------------+
                            |
                         enp0s8
                     192.168.100.10
                            |
                      LAB-NETWORK
                    192.168.100.0/24
                            |
                  VirtualBox Host-Only
                            |
                     192.168.100.1
                            |
                       Windows Host
```

### Network Interfaces

| System | Interface | Address | Purpose |
|---|---|---|---|
| Windows Host | VirtualBox Host-Only | `192.168.100.1/24` | LAB management |
| Ubuntu DEV01 | `enp0s3` | DHCP / `10.0.2.15/24` | Internet access through NAT |
| Ubuntu DEV01 | `enp0s8` | `192.168.100.10/24` | Internal LAB communication |

The NAT interface provides external connectivity, while the Host-Only interface
creates an isolated management network between the host and laboratory
machines.

No default gateway is configured on the Host-Only interface. Internet traffic
continues to use the NAT interface.

---

## Technology Stack

### Current

| Area | Technology |
|---|---|
| Host operating system | Windows |
| Virtualization | Oracle VirtualBox |
| Guest operating system | Ubuntu Desktop |
| Version control | Git |
| Repository hosting | GitHub |
| Remote administration | OpenSSH |
| Documentation | LaTeX |
| Editor / IDE | Visual Studio Code |

### Planned

The environment will gradually be expanded with technologies including:

| Area | Planned technologies |
|---|---|
| Containers | Docker / Docker Compose |
| Databases | PostgreSQL |
| Automation | Python, Bash |
| Infrastructure automation | Ansible |
| Monitoring | Prometheus, Grafana |
| Machine Learning | Python ML ecosystem |
| IoT / messaging | MQTT |
| Game infrastructure | Dedicated game servers |
| Security | Firewalling and network segmentation |
| CI/CD | GitHub Actions |

The exact technology stack may evolve as the project develops.

---

## Repository Structure

The repository is organized by infrastructure domain.

```text
homelab-infrastructure/
|
+-- .github/
|   +-- ISSUE_TEMPLATE/
|   +-- workflows/
|
+-- docs/
|   +-- latex/
|       +-- chapters/
|       +-- images/
|       +-- diagrams/
|       +-- bibliography/
|       +-- main.tex
|
+-- infrastructure/
|
+-- docker/
|
+-- database/
|
+-- automation/
|
+-- ml/
|
+-- game-servers/
|
+-- scripts/
|
+-- .gitignore
+-- README.md
+-- SECURITY.md
```

Not all directories contain production-ready components yet. They represent
the planned structure of the project as the laboratory grows.

---

## Documentation

Detailed technical documentation is maintained in LaTeX.

The documentation describes both the implementation and the reasoning behind
individual infrastructure decisions.

Current documentation covers:

1. Project introduction
2. Architecture
3. Virtual environment
4. Ubuntu installation
5. VirtualBox Guest Additions
6. Basic Linux configuration
7. Network configuration
8. Version control and repository management

The documentation includes:

- configuration procedures,
- command explanations,
- expected command results,
- architecture decisions,
- network configuration,
- verification procedures,
- troubleshooting notes,
- security considerations.

The LaTeX source is located in:

```text
docs/latex/
```

The main document can be compiled using:

```bash
latexmk -pdf main.tex
```

Generated LaTeX artifacts are excluded from version control.

---

## Development Workflow

Development follows a branch-based workflow.

Direct development on `main` is avoided.

The general workflow is:

```text
Issue
  |
  v
Development branch
  |
  v
Implementation
  |
  v
Testing
  |
  v
Commit
  |
  v
Push
  |
  v
Pull Request
  |
  v
Review
  |
  v
Merge
  |
  v
main
```

Example:

```bash
git switch main
git pull

git switch -c feature/example-change

# Implement and test the change

git add .
git commit -m "feat: implement example change"

git push -u origin feature/example-change
```

A Pull Request is then created against `main`.

After the Pull Request is merged:

```bash
git switch main
git pull

git branch -d feature/example-change
```

---

## Branch Naming Convention

Branches describe the type and purpose of a change.

Examples:

```text
feature/lab-network
feature/postgresql
feature/monitoring

docs/version-control
docs/network-verification

fix/virtualbox-network
```

Where possible, branches may also reference their associated GitHub Issue:

```text
feature/12-lab-network
```

---

## Commit Convention

Commit messages follow a simplified semantic convention inspired by
Conventional Commits.

### Feature

```text
feat: configure lab host-only network
```

### Bug fix

```text
fix: correct VirtualBox network configuration
```

### Documentation

```text
docs: add network verification tests
```

### Refactoring

```text
refactor: reorganize network configuration
```

### Repository / maintenance

```text
chore: configure repository rules
```

This convention makes the project history easier to understand and prepares
the repository for future automated release workflows.

---

## Repository Management

The `main` branch represents the stable integration state of the project.

Repository rules are designed to prevent accidental changes to the primary
branch.

The intended policy includes:

- Pull Requests for changes to `main`
- blocked force pushes
- protection against deletion of `main`
- resolved Pull Request conversations before merging
- dedicated development branches
- clean integration history

The project is currently developed by a single developer. Mandatory external
Pull Request approvals will be introduced when additional collaborators are
added.

Future repository controls will include:

- required Pull Request approvals,
- CODEOWNERS,
- required CI status checks,
- automated security checks,
- dependency scanning.

---

## Project Management

Development work is tracked using GitHub Issues, Milestones, and GitHub
Projects.

### Issues

Issues represent individual units of work such as:

- features,
- infrastructure changes,
- bugs,
- documentation tasks,
- security improvements.

An Issue should define the objective and acceptance criteria before
implementation begins.

Example:

```text
Issue #12
Configure Host-Only laboratory network

Acceptance criteria:

- enp0s8 configured
- static LAB address assigned
- Windows -> Ubuntu connectivity verified
- Ubuntu -> Windows connectivity verified
- SSH connectivity verified
- documentation updated
```

Pull Requests can reference Issues using:

```text
Closes #12
```

---

## Milestones

Milestones represent major development stages.

The current milestone is:

### M1 - Lab Foundation

The objective of this milestone is to establish the base infrastructure required
for further development.

It includes:

- VirtualBox environment
- Ubuntu installation
- VirtualBox Guest Additions
- Linux configuration
- network configuration
- SSH administration
- Git configuration
- repository management
- project documentation

Future milestones will be created as the corresponding project stages are
planned.

---

## Project Board

Development tasks are organized using the:

**Homelab Infrastructure Roadmap**

GitHub Project.

The project workflow follows:

```text
Backlog
   |
   v
Ready
   |
   v
In Progress
   |
   v
In Review
   |
   v
Done
```

This provides a high-level view of the current state of development work.

---

## Network Design

The initial laboratory network uses:

```text
192.168.100.0/24
```

Current address allocation:

| Address | Host | Purpose |
|---|---|---|
| `192.168.100.1` | Windows Host | VirtualBox Host-Only interface |
| `192.168.100.10` | Ubuntu DEV01 | Development / management |
| `192.168.100.20` | Reserved | Future infrastructure |
| `192.168.100.30` | Reserved | Future infrastructure |

Additional address ranges and network segments will be introduced as the
environment grows.

Future infrastructure may separate:

```text
Management
Servers
Databases
Game Servers
IoT
Monitoring
```

into dedicated network segments.

---

## Network Verification

Network configuration is not considered complete until it has been tested.

The current verification process includes:

### Interface configuration

```bash
ip -br addr
```

### Routing

```bash
ip route
```

### Host-to-VM connectivity

```powershell
ping 192.168.100.10
```

### VM-to-host connectivity

```bash
ping -c 4 192.168.100.1
```

### Layer 2 verification

Linux:

```bash
ip neigh show
```

Windows:

```powershell
arp -a
```

### SSH host verification

```bash
sudo ssh-keygen -lf /etc/ssh/ssh_host_ed25519_key.pub
```

The fingerprint can be compared with the fingerprint presented during the
first SSH connection.

### Internet routing

```bash
ping -c 4 8.8.8.8
```

### DNS resolution

```bash
ping -c 4 github.com
```

These tests verify different layers of the network rather than relying on a
single connectivity test.

---

## Security Principles

Security is treated as an integral part of the project and is introduced
incrementally as the infrastructure evolves.

Current principles include:

- SSH key-based authentication where appropriate
- private SSH keys are never committed
- credentials and secrets are excluded from version control
- infrastructure services are not exposed unnecessarily
- management traffic uses a dedicated Host-Only network
- changes to stable infrastructure are introduced through Pull Requests
- network configuration is verified before being considered complete
- public documentation is sanitized before being merged

### Public Documentation Policy

This repository is intended to be publicly accessible as part of the project
portfolio.

Technical information required to understand the architecture may therefore be
documented, including:

- private laboratory IP addresses
- private subnet definitions
- service ports
- network topology
- operating systems and technologies
- cryptographic algorithms and protocols

For example:

```text
LAB network:    192.168.100.0/24
Ubuntu DEV01:   192.168.100.10
SSH:            ED25519
```

These values describe the internal laboratory architecture and do not represent
authentication secrets.

Information identifying externally reachable infrastructure or containing
authentication material is not published.

Examples include:

```text
Public IP:        <PUBLIC_IP>
Public hostname:  <PUBLIC_HOSTNAME>
MAC address:      <MAC_ADDRESS>
SSH fingerprint:  SHA256:<REDACTED>
Password:         <NEVER COMMIT>
API token:        <NEVER COMMIT>
Private SSH key:  <NEVER COMMIT>
```

The general documentation policy is:

> Document architecture and mechanisms without unnecessarily exposing the
> identity or access details of the real infrastructure.

Sensitive information must never be committed to the repository, even if the
corresponding file pattern is included in `.gitignore`.

---

## Roadmap

The project will be developed incrementally.

```text
M1 - Lab Foundation
 |
 +-- Virtualization
 +-- Ubuntu
 +-- Networking
 +-- SSH
 +-- Git
 +-- Documentation
 |
 v
Container Platform
 |
 v
Database Platform
 |
 v
Monitoring & Observability
 |
 v
Automation
 |
 v
Machine Learning
 |
 v
Game Server Infrastructure
 |
 v
Network Segmentation & Security
```

The roadmap is intentionally flexible. Architectural decisions may change as
new requirements and technical constraints are discovered.

---

## Current Status

**Project status:** Active Development

Current focus:

```text
M1 - Lab Foundation
```

Implemented:

- [x] VirtualBox environment
- [x] Ubuntu Desktop installation
- [x] VirtualBox Guest Additions
- [x] Basic Linux configuration
- [x] Git configuration
- [x] GitHub SSH authentication
- [x] `main` branch migration
- [x] Branch-based development workflow
- [x] Initial LaTeX documentation
- [ ] Complete LAB network configuration
- [ ] Complete network verification
- [ ] Repository management configuration
- [ ] Issue and Pull Request templates
- [ ] CI pipeline

---

## Design Philosophy

The primary objective of this project is learning through implementation.

Instead of immediately relying on high-level automation, core infrastructure
components are first configured and verified manually.

The general approach is:

```text
Understand
   |
   v
Design
   |
   v
Configure
   |
   v
Test
   |
   v
Document
   |
   v
Automate
   |
   v
Improve
```

This makes it possible to understand not only how to deploy infrastructure,
but also how the underlying technologies interact.

---

## Author

**Adam Sypnik**

Homelab infrastructure and automation project.

---

## License

A license has not yet been selected for this project.