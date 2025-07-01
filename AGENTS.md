# Agent Guidelines

The following conventions apply to this repository.

## Testing

- Run `ansible-lint` and `ansible-playbook --syntax-check` for any change that modifies playbooks, roles or inventory files.
- Documentation-only changes do not require running tests.

## Development Notes

- Playbooks should support the `--check` flag for safe dry runs.
- Prefer existing container images for Synapse, PostgreSQL and Coturn rather than building custom images.
- Configuration should remain minimal; passwords and secrets may be generated automatically by the playbooks.

