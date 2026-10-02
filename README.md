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
built **mecmcp-native** on [`mecmcp`](https://github.com/mechubsec/mecmcp)
like [`rustjunosmcp`](https://github.com/mechubsec/rustjunosmcp) and
[`rustpanosmcp`](https://github.com/mechubsec/rustpanosmcp).

## Status

**Planned — on hold until a FortiGate lab device is available.** No code yet.

- **Phase 1:** FortiOS REST v2 (`/api/v2/cmdb`, `/api/v2/monitor`) with API
  tokens only. Reads for system status, policies, address and service objects,
  routes and sessions; writes limited to firewall policy and address objects
  through mecmcp change sets (plan, digest, human approval, apply).
- **Phase 2:** FortiManager JSON-RPC — ADOMs, policy packages,
  install-to-device.

Design and scope: [mecmcp#423](https://github.com/mechubsec/mecmcp/issues/423).

## Redaction

When implemented, this server **must** route all device and API output, including error text, through [`mecmcp-redact`](https://github.com/mechubsec/mecmcp/tree/main/crates/mecmcp-redact) via `redact_json_value_with_profile`, using a FortiOS `Profile` hook declared in this crate. The generic value-shape scan already catches `ENC`-prefixed ciphertext, PEM material and password hashes. The FortiOS profile adds wholesale withholding for vendor-rendered bodies and any key exemptions. No tool may return raw device config or API payloads.

Redaction must be applied at the boundary between the vendor read API and the tool result, not in the tool logic itself.

See [`mecmcp-redact`](https://github.com/mechubsec/mecmcp/tree/main/crates/mecmcp-redact) for the complete redaction policy, denylist, and value-shape catch-alls.

---

## License

Licensed under [MIT](LICENSE).
