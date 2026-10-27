# Director MCP server

Director records narrated product demo videos of your web app from one
sentence, and turns the map of your product it builds along the way into
help-center articles.

This repository describes Director's hosted MCP server. The server itself
runs at `https://api.director.screenforge.co/api/mcp`; there is nothing to
install or run locally.

- Website: https://director.screenforge.co
- App (sign up, API keys): https://app.director.screenforge.co
- Registry name: `co.screenforge/director`

## What it does

Give Director a URL and an instruction such as "show how to invite a
teammate". It drives a real browser through your app, records it, writes and
voices the narration, and returns a polished video. While it works it maps the
pages and flows it walks, and you can turn any of those flows into a
step-by-step help article with screenshots.

## Configure

The server uses Streamable HTTP. Sign in with OAuth (your client opens a
browser the first time), or send a Director API key from
app.director.screenforge.co in the `X-API-Key` header.

**VS Code / GitHub Copilot** (`.vscode/mcp.json`):

```json
{
  "servers": {
    "director": {
      "type": "http",
      "url": "https://api.director.screenforge.co/api/mcp"
    }
  }
}
```

To use an API key instead of OAuth:

```json
{
  "inputs": [
    {
      "type": "promptString",
      "id": "director-api-key",
      "description": "Director API key",
      "password": true
    }
  ],
  "servers": {
    "director": {
      "type": "http",
      "url": "https://api.director.screenforge.co/api/mcp",
      "headers": { "X-API-Key": "${input:director-api-key}" }
    }
  }
}
```

**Claude Code**:

```bash
claude mcp add --transport http director https://api.director.screenforge.co/api/mcp
```

**Any other MCP client**: add a remote (Streamable HTTP) server with the URL
above.

## Use

Ask your assistant in plain language, for example:

- "Record a demo of app.example.com showing how to create a project."
- "Is my demo done yet? Send me the video link."
- "List the flows Director found on app.example.com and write a help article
  for the billing one."
- "Publish that article."

Recording a site you don't own needs your confirmation that you're authorized
to demo it, or a verified domain. Apps behind a login use a saved environment
that holds the sign-in details, so you don't paste passwords into chat.

## Tools

**Demos**: `create_demo`, `wait_for_demo`, `get_demo_status`,
`get_demo_transcript`, `answer_demo_question`, `cancel_demo`, `list_demos`

**Environments (logged-in apps)**: `create_environment`, `get_environment`,
`update_environment`, `list_environments`, `verify_environment`,
`create_login_code`, `delete_session`

**Domains**: `list_domains`, `attest_domain`, `start_domain_verification`,
`check_domain_verification`

**Product map**: `list_graph_flows`, `get_graph_flow`, `reverify_flow`

**Help articles**: `list_articles`, `get_article`, `create_article_from_flow`,
`create_article`, `update_article`, `regenerate_article`, `publish_article`,
`unpublish_article`, `resync_article`, `delete_article`,
`list_article_categories`, `create_article_category`,
`create_article_section`

**Recording presets**: `list_recording_presets`, `create_recording_preset`,
`update_recording_preset`

**Integrations**: `list_integrations`

Every tool is annotated read-only or destructive so your client can ask before
anything changes.

## Support

Email support@screenforge.co.
