# Setting up Remote Desktop Commander

> Connect the Remote Desktop Commander MCP server in your AI assistant, then pair a machine so the assistant can control it remotely through a small local agent.

Remote Desktop Commander is a hosted MCP (Model Context Protocol) server. You add it to your AI assistant as a connector, and it relays to a small agent running on whatever machine you want the assistant to control — giving it that machine's files, terminal, search, and code tools.

Setup has two parts: pair the machine the assistant should reach (Step 1), then connect Remote Desktop Commander in your AI assistant (Step 2). That way your first tool call already has a device to reach.

## Requirements

- A **Remote Desktop Commander account** — you'll sign in (or sign up) with Google or email during setup.
- An **AI assistant that supports remote MCP connectors** (claude.ai, Claude Desktop, Claude Code, ChatGPT, Cursor, VS Code, Gemini CLI, and others).
- **Node.js 18.0.0 or newer** on the machine you want to control (the `npx` agent runs there). Check with `node --version`.

## Step 1: Pair the machine you want to control

Remote Desktop Commander relays through a small agent running on the machine you want to control. Pair that machine with a one-time device check:

1. On the machine you want to control, open a terminal and run:

   ```bash
   npx @wonderwhy-er/desktop-commander@latest remote
   ```

2. A browser window opens to a **Verify your device** page showing a pairing code. The **same code** is printed in your terminal — this is a standard OAuth device-flow check confirming the agent requesting access is the one you're approving.
3. **Sign in or sign up** with Google or email if you aren't signed in already.
4. Confirm the code in the browser **matches** the code in your terminal, then click **Verify Device**.
5. The terminal confirms the device is ready, and the machine appears under **Your Devices** on the dashboard.

**Leave this terminal window open while you're using Remote Desktop Commander.** The agent runs in the foreground, so closing the terminal disconnects the device.

## Step 2: Connect your AI assistant

1. In your assistant's connector or MCP settings, add a new connector with the server URL:

   ```
   https://mcp.desktopcommander.app/mcp
   ```

   Client-by-client instructions are in the [README](../README.md#quick-start). You can also open the **Connect an AI assistant** tab on the [dashboard](https://mcp.desktopcommander.app) for guided setup.

2. Your assistant starts an OAuth sign-in — the Remote Desktop Commander sign-in page opens in your browser.
3. **Sign in** with the same account you used when pairing.

Once you've authorized, your assistant is linked to your Remote Desktop Commander account and can reach your paired devices.

## Confirm it's working

Ask your assistant to do something on the paired machine — for example: *"Using Remote Desktop Commander, list the files on my Desktop."* The assistant will call the tool on your device and report back. If it lists your files, the connection is working.

## Manage and revoke devices

The [dashboard](https://mcp.desktopcommander.app) lists your connected machines under **Your Devices**, with each one's online status and last-seen time. From there you can:

- **Add a device** — shows the `npx … remote` command to pair another machine.
- **Connect an AI assistant** — shows setup instructions for each supported client.
- **Revoke** a single device, or **Revoke All**, to immediately cut off access.

## Troubleshooting

**The device shows offline, or the assistant can't reach it:**

- Make sure the terminal running `npx @wonderwhy-er/desktop-commander@latest remote` is still open on the target machine. Closing it disconnects the device — re-run the command to reconnect.

**The pairing codes don't match:**

- Don't click Verify Device. Close the page, re-run the `remote` command, and only verify when the browser code matches the terminal code. The match is a security check.

**Authorization didn't finish:**

- Make sure you completed sign-in on the Remote Desktop Commander page after your assistant opened the authorization window.

**The `remote` command fails to run:**

- Make sure Node.js 18+ is installed on the target machine (`node --version`).

**A tool call is blocked or fails after the connection succeeds:**

- Use the [tool-call diagnostics guide](TOOL_CALL_DIAGNOSTICS.md) to distinguish client approvals, safety blocks, authentication errors, agent restrictions, and process failures. Preserve the exact result and use the normal approval or support route.
