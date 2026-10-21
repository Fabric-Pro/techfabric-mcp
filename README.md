<p align="center">
  <img src="icon.png" alt="TechFabric mark" width="96" height="96">
</p>

# techfabric.com MCP server

[TechFabric](https://www.techfabric.com)'s engineers write blog posts and case
studies from the coding agent they already use, with the repository and context
it already has. This MCP server is how: the agent takes today's brief, runs the
site's checks, files the draft under the engineer's byline and, once the
engineer has read the preview and said yes, publishes it.

Anyone can use it to search and read the site, without signing in.

```
https://www.techfabric.com/mcp
```

It's a remote server over Streamable HTTP, so there's nothing to install, and
it's free.

## What's in this repository

Descriptor files, so MCP directories have a repository to list. The server's
code isn't here and isn't open source.

| File | What it is |
| --- | --- |
| `server.json` | The server's entry in the [official MCP Registry](https://registry.modelcontextprotocol.io/v0.1/servers/com.techfabric%2Ftechfabric/versions/latest), copied as published |
| `glama.json` | Maintainer list for [Glama](https://glama.ai/mcp/connectors/com.techfabric/techfabric) |
| `icon.png`, `icon-light.png` | The TechFabric mark at 256 px, for dark and light backgrounds |
| `LICENSE` | Apache-2.0, for these files only |

The live server and the registry are the source of truth. If this README and
the server's `tools/list` ever disagree, the server is right. A weekly GitHub
Action compares `server.json` with the registry and opens a pull request when
the entry has changed.

## Add it to your agent

### Claude Code

```sh
claude mcp add --transport http techfabric https://www.techfabric.com/mcp
```

Add `--scope user` to have it in every project. TechFabric authors should
install the [Claude Code plugin](https://github.com/Fabric-Pro/techfabric-claude-plugin)
instead, which adds the server, the seven commands and a writing skill in one
go, and asks before every publish:

```sh
claude plugin marketplace add Fabric-Pro/techfabric-claude-plugin && claude plugin install techfabric@techfabric
```

### Claude Desktop and claude.ai

Open **Customize**, then **Connectors**, choose **Add** and then **Add custom
connector**. Name it techfabric, paste `https://www.techfabric.com/mcp`, keep
the authentication settings Claude detects, and choose **Add**. On a Team or
Enterprise plan, an owner may have to add the connector for the organisation
first.

### ChatGPT

At [chatgpt.com/plugins](https://chatgpt.com/plugins), choose **+** and then
**Add custom MCP server**. Name it techfabric, paste the URL under
**Connection**, choose OAuth for authentication, and create it as a plugin. In a
chat, type `@` and pick techfabric. If **Add custom MCP server** isn't there,
your workspace's admin has to turn it on.

### Codex

```sh
codex mcp add techfabric --url https://www.techfabric.com/mcp
```

Codex may start a sign-in when you add it. Readers can skip it, because every
read tool works without one. By hand, in `~/.codex/config.toml`:

```toml
[mcp_servers.techfabric]
url = "https://www.techfabric.com/mcp"
```

### Cursor

In `~/.cursor/mcp.json` for every project, or `.cursor/mcp.json` for one:

```json
{
  "mcpServers": {
    "techfabric": { "url": "https://www.techfabric.com/mcp" }
  }
}
```

### VS Code

In `.vscode/mcp.json`, then start it from that file or the MCP view. The tools
show up in Copilot's agent mode, and the commands as `/techfabric.write-post`
and so on in Copilot Chat.

```json
{
  "servers": {
    "techfabric": { "type": "http", "url": "https://www.techfabric.com/mcp" }
  }
}
```

### Anything else

Point the client at the URL with the Streamable HTTP transport. Gemini CLI,
GitHub Copilot CLI, OpenCode and Fabric Agents each have their own steps in the
[setup guide](https://www.techfabric.com/mcp-servers#set-up).

## Tools

Four read tools work for anyone:

| Tool | What it does |
| --- | --- |
| `search` | Searches the published site (blog posts, service and solution pages, case studies, city pages and events), best first |
| `get_page` | Reads one published page as Markdown, with its title, description, dates, author, tags and FAQs |
| `list_posts` | Lists blog posts, newest first, by tag, author or date |
| `get_services` | Lists every service, solution, accelerator and product page, with the summary the live page gives |

They answer only from published, indexable pages, so drafts and previews never
show up in a result.

Eight author tools need a TechFabric engineer's Microsoft sign-in. Called
anonymously, each answers with the sign-in challenge.

| Tool | What it does |
| --- | --- |
| `get_brief` | Today's topic for a daily draft, held for the signed-in engineer for 24 hours |
| `lint_draft` | Runs the site's rule-based checks on a draft's Markdown at once, each finding naming its passage |
| `check_draft` | Runs the site's full readiness checks, the same ones the browser editor runs before publishing |
| `submit_draft` | Files a post or case study into the blog pipeline under the engineer's own byline |
| `update_draft` | Saves a new version of one of the engineer's own drafts |
| `get_status` | Says where one of your drafts stands: its state, what's failing and its preview |
| `preview_draft` | Shows a draft as techfabric.com will render it, as an interactive app in hosts that support [MCP Apps](https://modelcontextprotocol.io/) |
| `publish_draft` | Publishes one of your own drafts once its checks pass, and only after you confirm by the post's name |

## Commands

The server ships seven prompts, which most agents show as slash commands:
`write-post`, `write-case-study`, `check`, `preview`, `publish`, `revise` and
`status`. Each gives the agent the steps in order and points it at the house
rules. In Claude Code with the plugin they're `/techfabric:write-post` and so
on. The [setup guide](https://www.techfabric.com/mcp-servers#commands) has the
equivalents for Codex, Cursor, Gemini CLI and Fabric Agents.

## Things to ask

Anyone:

- "What does TechFabric do with Databricks? Use the techfabric MCP server."
- "Find TechFabric's blog posts about MCP and summarise the newest one."
- "Read techfabric.com's page on cloud migration and list the questions its
  FAQ answers."

A signed-in TechFabric author:

- `/techfabric:write-post how we cut our Lakeflow job's cold start`
- "Write a TechFabric blog post about how we <the problem you solved>. Ask me
  for anything you don't know instead of inventing it, and give me the preview
  link when it's filed."
- `/techfabric:status`

## Sign-in, limits and privacy

Reading needs no account and sets no cookie. The author tools are for
TechFabric's own blog authors, who sign in with their Microsoft account in the
techfabric.com tenant and must be on the blog's author list. No other tenant is
accepted, and a sign-in lasts up to 30 days.

Anonymous callers get 300 calls an hour per IP address. Signed-in authors get
600 an hour, with lower caps on check runs, submissions, saves and publishes.
Past a limit the server answers HTTP 429 with a `Retry-After` header.

Neither the read tools nor the log keep what you searched for or read: a log
line names the tool and the protocol revision, never its arguments, and no read
call reaches a language model. When an author signs in to write, the server
keeps the text they check, file or save, with their address against it. The
[privacy policy](https://www.techfabric.com/privacy-policy) covers the rest.

It speaks MCP revision 2026-07-28, and clients still on 2025-11-25 are served on
the same URL. In the official registry it's `com.techfabric/techfabric`.

## Links

- [Setup guide for both TechFabric MCP servers](https://www.techfabric.com/mcp-servers#set-up)
- [Claude Code plugin](https://github.com/Fabric-Pro/techfabric-claude-plugin)
- [The Databricks Field Guide MCP server](https://github.com/Fabric-Pro/databricks-field-guide-mcp), TechFabric's other one, for Databricks questions
- [TechFabric's blog](https://www.techfabric.com/blog)
- [What TechFabric does](https://www.techfabric.com/services)
- [Talk to an engineer](https://www.techfabric.com/contact)

## License

The files in this repository are under the [Apache License 2.0](LICENSE),
copyright Tech Fabric LLC. The site's content and the server itself aren't
covered by it.
