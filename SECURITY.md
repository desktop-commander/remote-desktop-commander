# Security Policy

Remote Desktop Commander relays tool calls from AI applications to computers our users own. We take the security of that path seriously and appreciate responsible disclosure.

## Security model

Remote Desktop Commander gives an AI assistant real file and terminal access on the machines you pair. Understanding the trust model matters:

- **Tools execute with your user account's permissions** on the paired machine. The service is an amplifier of whatever the connected AI client asks it to do — it can do anything your account can do.
- **The connected AI account is trusted.** Anyone who can drive your connected AI assistant (or your Remote Desktop Commander account) can reach your paired machines. Protect both with strong credentials and multi-factor authentication.
- **Built-in restrictions are guardrails, not a sandbox.** Configuration options like allowed directories and command blocking reduce accidental or unintended actions; they are not a security boundary against a malicious or compromised client.
- **You are in control of exposure.** A machine is reachable only while its device agent is running — stop it with `Ctrl+C` and access ends. Devices can be revoked at any time (individually or all at once) from the [dashboard](https://mcp.desktopcommander.app).
- **Pairing is verified.** Device pairing uses the OAuth device authorization flow: only approve a pairing code in the browser if it matches the code shown in your own terminal.
- **Transport is encrypted in transit** (HTTPS/TLS) and all relayed calls are scoped to your account.
- **Relayed data is transient.** Tool calls and their results pass through the hosted relay to deliver responses and are not retained once delivered; the service collects operational telemetry (event metadata such as tool name and success/failure — not file contents) as described in the [Remote Desktop Commander privacy policy](https://legal.desktopcommander.app/#/remote_privacy_policy).

**Recommendations:** pair only machines you own; keep the agent running only when you need it; use MFA on your AI assistant and Remote Desktop Commander accounts; for high-risk or experimental work, run the agent inside a container or VM.

## Reporting a vulnerability

**Please do not report security vulnerabilities through public GitHub issues.**

Instead, use one of:

- **GitHub private vulnerability reporting** — [Report a vulnerability](https://github.com/desktop-commander/remote-desktop-commander/security/advisories/new) on this repository (preferred)
- **Email** — legal@desktopcommander.app

Please include a description of the issue, steps to reproduce, and the potential impact. We will acknowledge your report within 3 business days.

## Scope

- The hosted service at `mcp.desktopcommander.app` / `auth.desktopcommander.app` (OAuth flows, MCP endpoint, device pairing, session handling)
- The device agent (`npx @wonderwhy-er/desktop-commander remote`) — also reportable via the [DesktopCommanderMCP](https://github.com/wonderwhy-er/DesktopCommanderMCP) repository, whose [security policy](https://github.com/wonderwhy-er/DesktopCommanderMCP/blob/main/SECURITY.md) covers the underlying open-source tool engine

## Please avoid

- Accessing or modifying data belonging to other users
- Degrading the service for others (e.g. volumetric testing against production)
- Social engineering of Desktop Commander team members
