# Security Policy

## Public Repository

This repository is publicly accessible.

All contributions must be reviewed for sensitive information before being
merged.

## Never Commit

The following information must never be committed:

- passwords
- API keys
- access tokens
- private SSH keys
- database credentials
- `.env` files containing secrets
- connection strings containing credentials
- public infrastructure IP addresses when not required
- other authentication material

## Documentation

Private laboratory addressing may be documented when required to explain the
architecture.

Public infrastructure identifiers should be replaced with placeholders:

```text
<PUBLIC_IP>
<PUBLIC_HOSTNAME>
<MAC_ADDRESS>
SHA256:<REDACTED>
```

## SSH

SSH algorithms and authentication mechanisms may be documented.

Private SSH keys must never be stored in this repository.

## Reporting Security Issues

Security issues discovered in this project should not be disclosed through a
public GitHub Issue if they contain credentials or information that could expose
the infrastructure.