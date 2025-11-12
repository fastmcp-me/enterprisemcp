# Microsoft MCP Server for Enterprise

## Overview

Built on the open [Model Context Protocol](https://modelcontextprotocol.io), the **Microsoft MCP Server for Enterprise** lets AI agents access **Microsoft Entra** data by converting natural language queries into Microsoft Graph API calls.
This MCP server empowers developers and IT administrators to integrate the management of organizational data into AI-powered workflows.

## Quick Start

To get started with the Microsoft MCP Server for Enterprise, follow these steps:

1. Install Microsoft.Entra.Beta PowerShell module:

   ```powershell
   Install-Module Microsoft.Entra.Beta -Force -AllowClobber
   ```

2. Connect Microsoft Entra ID to the tenant you'd like to register the MCP Server:

   ```powershell
   Connect-Entra -Scopes 'Application.ReadWrite.All', 'DelegatedPermissionGrant.ReadWrite.All'
   ```

3. Run this cmdlet to register the MCP Server for Enterprise in your tenant and grant all permissions to Visual Studio Code:

   ```powershell
   Grant-EntraBetaMCPServerPermission -ApplicationName VisualStudioCode
   ```

4. Click [Install Microsoft MCP Server for Enterprise](https://vscode.dev/redirect/mcp/install?name=Microsoft%20MCP%20Server%20for%20Enterprise&config=%7b%22name%22:%22Microsoft%20MCP%20Server%20for%20Enterprise%22%2c%22type%22:%22http%22%2c%22url%22:%22https://mcp.svc.cloud.microsoft/enterprise%22%7d) to launch VS Code's MCP install page.

5. Click the Install button in VS Code and Login with your Admin account from the tenant above

6. Open Copilot Chat and ask a question about your tenant.

You will be able to use your own MCP Client by configuring it with the script
`Grant-EntraBetaMCPServerPermission -ApplicationId <Your_Application_Id>`.

## Tools

This MCP Server is atypical, as it doesn't wrap Microsoft Graph requests into individual tools; but it uses Retrieval-augmented generation (RAG) and few-shot technique to generate a full Microsoft Graph query.  
The MCP Server provides three main tools to facilitate this process:

1. **microsoft_graph_suggest_queries:** Finds relevant Microsoft Graph API calls based on user intent.
2. **microsoft_graph_get:** Executes read-only Microsoft Graph API calls, respecting User roles and MCP Client scopes.
3. **microsoft_graph_list_properties:** Retrieves properties of specific Microsoft Graph entities to help the AI model

## Current scope and capabilities

For **Private Preview**, our focus is to support Read-only enterprise IT scenarios focused on Microsoft Entra identity and directory operations (user, group, application, device management, and administrative actions).

In particular, the MCP Server can handle queries related to:

1. **Security posture**: authentication methods/strengths, Conditional Access, Security Defaults.
2. **Privileged access**: Who has which directory roles, how assigned (direct vs group), and PIM status.
3. **Application risk**: Which Apps / Service Principals exist, who owns them, what permissions/SSO they use, and which are ownerless or external.
4. **Access governance**: Who has access to what (users, groups, packages); review decisions, automate joiner/mover/leaver.
5. **Device readiness**: Managed/compliant status, join state, OS/version distribution, and stale or inactive devices.
6. **Provenance and investigation**: End‑to‑end telemetry (sign‑in, audit, provisioning, network), health alerts, and SLA/availability.
7. **Optimize spending & hygiene**: License counts/usage, unused or stale apps/groups, domain configuration and contacts.

## Licensing and costs

- The MCP Server for Enterprise **doesn't require extra cost or separate license**.
- You need the right licenses for the data you access (for example, Microsoft Entra ID P2 for sign-in logs).
- Follows Microsoft Graph API throttling limits.

## Cloud availability

The Microsoft MCP Server for Enterprise is available only in the public cloud (global service).

## Logs

To monitor usage, enable [Microsoft Graph activity logs](../microsoft-graph-activity-logs-overview.md) in your tenant. The system logs all API calls made through the MCP server.

**Filter for MCP Server usage:**

Use the appId (application/client ID) of the MCP server: `e8c77dc2-69b3-43f4-bc51-3213c9d915b4`.

The following sample Kusto query retrieves these logs:

```kusto
MicrosoftGraphActivityLogs
| where TimeGenerated >= ago(30d)
| where AppId == "e8c77dc2-69b3-43f4-bc51-3213c9d915b4"
| project RequestId, TimeGenerated, UserId, RequestMethod, RequestUri, ResponseStatusCode
```

## Support and reference

For documentation, troubleshooting, and feedback, refer to the official Microsoft Learn documentation and support channels.

## Security and compliance

All operations respect Microsoft Graph permissions and security policies.
Ensure compliance with your organizational, regulatory, and contractual requirements when integrating the MCP Server.

## No warranty/limitation of liability

This software is provided "as is" without warranties or conditions of any kind, either express or implied. Microsoft isn't liable for any damages that result from use, misuse, or misconfiguration of this software.
