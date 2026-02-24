# TidyD

This document outlines the planned development phases for tidyd, a fast, memory-safe file organizer daemon for Linux built by **Hapkeite**.

Our goal is to evolve `tidyd` from a simple script replacement into a robust, configurable, and invisible background service that Linux power users can trust.

---

## Phase 1: The Core Engine
*Goal: Prove the concept. Watch a directory and move a file safely.*

- [ ] **Basic File Watching:** Implement the `notify` crate to listen to a single, hardcoded directory (e.g., `~/Downloads`).
- [ ] **Event Filtering:** Ignore `Create` events to prevent file corruption; react only to `CloseWrite` events so files are fully downloaded before moving.
- [ ] **Simple Action Execution:** Move files to a hardcoded destination based strictly on their file extension (e.g., `.pdf` to `~/Documents`).
- [ ] **Terminal Logging:** Use the `tracing` crate to output structured logs to standard output for real-time debugging.

## Phase 2: Configuration & System Integration (v0.5.0)
*Goal: Make it universally useful by adding configurations and daemon capabilities.*

- [ ] **TOML Configuration:** Read user-defined rules, source directories, and target directories from `~/.config/tidyd/config.toml`.
- [ ] **Daemonization:** Ensure the binary can run completely detached from a terminal session.
- [ ] **Systemd Service:** Provide a standard `tidyd.service` template for easy Linux service management (`systemctl enable --now tidyd`).
- [ ] **Persistent Logging:** Route logs to standard Linux locations (e.g., `journalctl` or `~/.local/state/tidyd.log`).

## Phase 3: Advanced Rules & Actions (v1.0.0)
*Goal: Deliver the flexibility expected from a native, production-ready Linux tool.*

- [ ] **Regex Pattern Matching:** Allow users to match complex filenames using regular expressions (e.g., `^invoice_.*\.pdf$`).
- [ ] **Conflict Resolution:** Implement fallback rules if a file already exists at the destination (`Overwrite`, `Rename/Append`, or `Skip`).
- [ ] **Time-based Actions:** Process files based on their age (e.g., "Delete files in `~/Downloads/tmp` older than 7 days").
- [ ] **Archive Extraction:** Automatically extract `.zip` or `.tar.gz` files to a designated directory upon download.
- [ ] **Live Config Reloading:** Allow the daemon to reload its configuration automatically if `config.toml` is modified, without requiring a service restart.

## Phase 4: The Ecosystem (v2.0.0+)
*Goal: Expand from a single background daemon to a complete tool suite.*

- [ ] **CLI Companion (`tidydctl`):** Build a command-line interface to pause the daemon, check its status, or manually trigger a sort of a specific folder.
- [ ] **Desktop Notifications:** Integrate with D-Bus to send native Linux desktop notifications upon successful rules execution.
- [ ] **Execution Hooks:** Allow the execution of custom bash scripts before or after a file operation (e.g., running a ClamAV scan before moving a file).