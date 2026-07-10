# Security Policy

## Reporting a Vulnerability

If you believe you've found a security issue in this plugin or in the Zillapi service, email **hello@zillapi.com**. Please do not open a public issue for security reports.

We aim to acknowledge reports within 2 business days.

## Scope

- This repository contains only plugin metadata, prompt/skill markdown, and an MCP client configuration pointing at the hosted server `https://api.zillapi.com/mcp`.
- It contains **no secrets and no executable code**. Authentication is performed by your AI client via OAuth 2.1 directly against Zillapi — no API keys are stored in this repository or in your client configuration.
