# DeployMatrix Project Plan

DeployMatrix aims to provide an Ansible-based installation of a full Matrix communication stack using rootless Podman containers. The playbooks target RHEL compatible hosts and focus on minimal configuration while supporting both connected and air‑gapped environments.

## Components

The deployment provisions the following services:

- **Synapse** – Matrix homeserver.
- **PostgreSQL** – database backend for Synapse.
- **Coturn** – TURN server for media relay.
- **Element Web** – web interface for chat.
- **Element Call** – WebRTC calling client.
- **Keycloak** – optional SSO provider.

Server-side services (Synapse, PostgreSQL, Coturn) run inside a single container. Client-facing services (Element Web and Element Call) run in a separate container. Both containers are managed by Podman in rootless mode.

## Logging

All chat history and administrative actions are forwarded to the host's `rsyslog`. This ensures that message logs and audit logs are available for central collection without exposing the containers.

## Authentication

Authentication is provided through Keycloak, with user accounts stored in Active Directory via LDAPS. This integration can be disabled if AD is not available.

## Air‑Gapped Support

The playbooks are designed so that required container images and dependencies can be gathered in advance and bundled with the repository. Once copied to the target environment, the playbook can install the full stack without external network access.

## Minimal Configuration

Most variables have sensible defaults. Wherever possible, credentials (such as PostgreSQL passwords) are generated automatically. Prebuilt container images are used in place of custom builds to further reduce configuration requirements.

## Deployment Usage

1. Clone this repository to your control machine.
2. Edit the example inventory under `inventory/` to define your hosts.
3. Run the playbook:
   ```bash
   ansible-playbook -i inventory/hosts deploymatrix.yml
   ```
   To perform a dry run, add `--check`.

The playbook supports running on the local machine or targeting a remote server via SSH. Because containers are rootless, the same playbook can be executed under a normal user account or a dedicated service account.

## Linting and Tests

This project uses `ansible-lint` and `ansible-playbook --syntax-check` to validate playbooks. Continuous integration will run these checks automatically. When making documentation‑only changes, testing is optional.

## Project Status

This repository now includes a working `deploymatrix.yml` playbook, sample inventory and CI workflows. Future updates may refine configuration and add more extensive documentation.

