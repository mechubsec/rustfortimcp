<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="docs/assets/mechub-mark.svg">
    <img src="docs/assets/mechub-mark-light.svg" width="72" alt="mechub mark">
  </picture>
</p>

<h1 align="center">rustfortimcp</h1>

<p align="center"><strong>Enterprise MCP server for Fortinet FortiGate — curated tools, scoped access, audited change control</strong><br>
<em>a mechub project — sovereign network-security automation</em></p>

---

`rustfortimcp` will be the Fortinet member of the mechub MCP server family: a
curated, scoped, audited MCP surface over FortiOS (and later FortiManager),
built **mecmcp-native** on [`mecmcp`](https://github.com/fastrevmd-lab/mecmcp)
like [`rustjunosmcp`](https://github.com/fastrevmd-lab/rustjunosmcp) and
[`rustpanosmcp`](https://github.com/fastrevmd-lab/rustpanosmcp).

## Status

**Planned — on hold until a FortiGate lab device is available.** No code yet.

- **Phase 1:** FortiOS REST v2 (`/api/v2/cmdb`, `/api/v2/monitor`) with API
  tokens only. Reads for system status, policies, address and service objects,
  routes and sessions; writes limited to firewall policy and address objects
  through mecmcp change sets (plan, digest, human approval, apply).
- **Phase 2:** FortiManager JSON-RPC — ADOMs, policy packages,
  install-to-device.

Design and scope: [mecmcp#423](https://github.com/fastrevmd-lab/mecmcp/issues/423).

## Redaction

This server routes **all device and API output through the shared [`mecmcp-redact`](https://github.com/mechubsec/mecmcp/tree/main/crates/mecmcp-redact) crate** using the FortiOS-specific [`Profile`](https://github.com/mechubsec/mecmcp/blob/main/crates/mecmcp-redact/src/profile.rs).

The profile ensures that FortiOS-specific secret patterns (including the `ENC` prefixed ciphertext markers used in CLI output and API responses) are redacted before any content reaches the model. This includes PSK secrets, API tokens, password hashes, and private keys embedded in device configurations.

Redaction is applied at the boundary between the vendor read API and the tool result, not in the tool logic itself. No tool may return raw device config or API payloads — every output path goes through mecmcp-redact.

See [`mecmcp-redact`](https://github.com/mechubsec/mecmcp/tree/main/crates/mecmcp-redact) for the complete redaction policy, denylist, and value-shape catch-alls.

---

## License

Licensed under [MIT](LICENSE).
