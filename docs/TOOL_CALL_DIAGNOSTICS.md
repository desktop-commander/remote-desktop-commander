# Diagnosing tool calls across the client, service, and device

A tool call passes through several stages: the AI client's authorization and
safety checks, the hosted Remote MCP service, the paired device agent, and the
requested local process or downstream service. Record the last stage supported
by evidence before changing settings. An application permission does not override
client safety checks or the device account's operating-system permissions.

## Classify the visible outcome

| Visible outcome | Evidence to collect | Next step |
| --- | --- | --- |
| Client displays an explicit safety block and returns no device result | Exact message, timestamp and timezone, tool name, sanitized arguments, approval-card availability, any client request ID | Preserve the failure and contact the relevant client/service support channel. |
| Client displays an approval card | Requested action and available approval controls | Review the action and use the client's normal approval flow. |
| Authentication fails or the connection expires | Authentication error and relevant connection status | Follow the normal sign-in or reconnection flow. |
| Device agent rejects a path or command | Agent error and the relevant configured restriction | Check that the requested operation is within the intended authorized scope. |
| A process starts and then fails | Returned PID, stdout/stderr excerpts, exit code, timeout status | Diagnose the local process or downstream service using its actual error. |

A missing PID or an absent local-history entry does not by itself prove that the
hosted service never received a request. Local history can be bounded and is not
a substitute for service-side correlation. Write "unknown" for unobserved stages.

## Record the version that was actually running

When available, record the version reported by `get_config` or the device agent's
startup output. Keep the agent version, client version, and any hosted deployment
identifier in separate fields. Record "unknown" when a field is unavailable.

`npm view @wonderwhy-er/desktop-commander version` reports the version currently
published in the npm registry. It does not establish which version is running on
the paired device. Keep registry lookups separate from runtime evidence.

## Build a minimal, sanitized report

Record one entry for each attempted action, including successful controls:

```text
Timestamp and timezone:
AI client and version, if exposed:
Running device-agent version and source of that value:
Hosted deployment version, if exposed:
Tool name:
Sanitized arguments and intended effect:
Application permission setting at the time:
Exact client-visible result:
Approval card available: yes / no / unknown
Client request/correlation ID: value / unavailable
Hosted service receipt: confirmed / absent with evidence / unknown
Device receipt: confirmed / absent with evidence / unknown
Process PID and exit status, if returned:
Configuration changes between attempts:
```

A successful control establishes only that action's result. A later success with
changed arguments is not an identical-request reproduction and does not identify
a root cause. An open issue or a description-only pull request is a diagnostic
lead, not evidence that a specific hosted-client failure has been fixed.

After an explicit safety refusal, preserve the result and use the normal approval
or support route. Do not encode a command, split a refused action across tools,
change accounts to bypass a refusal, or broaden filesystem/credential permissions
as a diagnostic shortcut. Keep the original intended action visible to reviewers.

## Share only the information needed for diagnosis

Before posting publicly, remove credentials, OAuth tokens, cookies, authorization
headers, private key material, internal hostnames/IPs, usernames, absolute project
paths, client/device identifiers, and unrelated command output. Use placeholders
such as `<authorized-host>` and `<project>/receipt.json`. Provide sensitive request
IDs or original logs only through an appropriate private support channel.

Do not attach the complete `get_config` response or tool history by default: these
may contain device/account identifiers, allowed paths, or other projects' activity.
For security vulnerabilities, use [SECURITY.md](../SECURITY.md) rather than a public
issue. Browser network archives can contain secrets even after automatic sanitizing;
review them before sharing and provide them only when support requests them.

## References

- [ChatGPT app permissions and action approvals](https://help.openai.com/en/articles/20001495)
- [Contacting OpenAI support and collecting diagnostic information](https://help.openai.com/en/articles/6614161)
- [npm view: registry information](https://docs.npmjs.com/cli/v11/commands/npm-view/)
- [Remote Desktop Commander setup](SETUP.md)

This guide improves diagnosis and reporting. It does not change client safety
policies, OAuth scopes, command execution, or the hosted service implementation.
