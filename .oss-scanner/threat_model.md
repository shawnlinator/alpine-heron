# Threat model

## What this project does and where untrusted input enters
Docker build recipe and entrypoint scripts for a qBittorrent container (linuxserver style).
Untrusted input: container environment variables, bind-mounted volumes, and the container's
network. The scripts in this repo are the code under review; the qBittorrent binary itself is
fetched at build time and is out of scope here.

## Components that matter most / least
- most: the entrypoint/startup shell scripts and any code that assembles commands from
  environment variables (command-injection surface).
- least: the Dockerfile itself and static config.

## How to exercise it
Inspect the shell scripts; trace how each ENV/mount is interpolated into commands. Run the
entrypoint in a sandbox with hostile environment values.

## How you rate severity
- command injection or path traversal via environment variables: high (critical if it leads to
  code execution on the host).
- information disclosure of secrets via logs: medium.

## Anything to leave alone
- The fetched third-party qBittorrent binary is out of scope for this repo.
