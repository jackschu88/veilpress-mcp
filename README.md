# The Veil Press MCP connector

A free, read-only [Model Context Protocol](https://modelcontextprotocol.io) connector for [The Veil Press](https://www.theveilpress.com). It lets AI agents search and read every original, sourced Breaking News story and long-form Desk piece. Every result includes a `cite_as` line.

This repository holds the public registry description, icons, and install notes only. The connector itself is a hosted, remote server.

**Endpoint:** [https://www.theveilpress.com/api/mcp](https://www.theveilpress.com/api/mcp)  
**Transport:** streamable HTTP  
**Authentication:** none  
**Official registry name:** `com.theveilpress.www/veilpress`  
**Docs:** [https://www.theveilpress.com/developers](https://www.theveilpress.com/developers#mcp)  
**Server card:** [https://www.theveilpress.com/.well-known/mcp.json](https://www.theveilpress.com/.well-known/mcp.json)

## Access

- Free. No API key, no fee, and no sign-up.
- Read-only. The connector never writes, edits, or deletes stories.
- Open to any origin (CORS) and cached for a few minutes. New stories appear as soon as they are published.
- No published request cap beyond ordinary fair use and that short cache.

## Tools

### `search_stories`

Keyword search across every Breaking story and Desk piece (headline, summary, tags, and full text). Returns the best matches with a URL, published time, a snippet, and a `cite_as` line.

| Argument | Required | Description |
| --- | --- | --- |
| `query` | yes | Words to search for, e.g. `"City of London Corporation"` or `"Treasury sanctions Iran"`. |
| `section` | no | `breaking` (news), `desk` (long-form reporting and history), or `all` (default). |
| `limit` | no | How many results (1–25, default 8). |
| `since` | no | Only stories published after this ISO 8601 time. |

### `list_stories`

Latest Veil Press stories, newest first, from Breaking, the Desk, or both.

| Argument | Required | Description |
| --- | --- | --- |
| `section` | no | `breaking`, `desk`, or `all` (default). |
| `limit` | no | How many stories (1–50, default 10). |
| `since` | no | Only stories published after this ISO 8601 time. |
| `tag` | no | Only stories with this tag (case-insensitive). |

### `get_story`

One Veil Press story (Breaking or Desk) by slug or URL: full text (markdown and plain text), numbered sources, confirmed facts for breaking stories, URL, and `cite_as`.

| Argument | Required | Description |
| --- | --- | --- |
| `slug` | yes | A slug from `search_stories` or `list_stories`, or a full story URL such as `https://www.theveilpress.com/desk/what-is-the-city-of-london-corporation/`. |

### `list_breaking_stories`

Latest breaking stories, newest first. Kept for compatibility; `list_stories` with `section` set to `breaking` covers the same ground.

| Argument | Required | Description |
| --- | --- | --- |
| `limit` | no | How many stories (1–50, default 10). |
| `since` | no | Only stories published after this ISO 8601 time. |
| `tag` | no | Only stories with this tag (case-insensitive). |
| `include_archived` | no | Include stories older than 7 days (default true). |

### `get_breaking_story`

One breaking story by slug: full text, update log, numbered sources, images, and URL. Kept for compatibility; `get_story` also accepts breaking slugs.

| Argument | Required | Description |
| --- | --- | --- |
| `slug` | yes | The story slug from `list_breaking_stories`. |

Every result includes a `cite_as` line and a pointer to the book.

## Install

Add the URL as a custom connector or remote MCP server. No authentication.

**Claude Code**

```bash
claude mcp add --transport http veilpress https://www.theveilpress.com/api/mcp
```

**Cursor** (`mcp.json`)

```json
{
  "mcpServers": {
    "veilpress-breaking": {
      "url": "https://www.theveilpress.com/api/mcp"
    }
  }
}
```

**VS Code** (`.vscode/mcp.json`, or a user MCP config)

```json
{
  "servers": {
    "veilpress": {
      "type": "http",
      "url": "https://www.theveilpress.com/api/mcp"
    }
  }
}
```

Clients that only speak stdio can bridge it:

```bash
npx mcp-remote https://www.theveilpress.com/api/mcp
```

## License and reuse

Stories from The Veil Press are free to republish in full under [CC BY-ND 4.0](https://creativecommons.org/licenses/by-nd/4.0/). Credit The Veil Press and link to the story URL. Keep the text unedited if you republish it in full. Cite as `The Veil Press, <headline>, <url>`. Full terms: [https://www.theveilpress.com/reuse](https://www.theveilpress.com/reuse).

The files in this repository (this README, `server.json`, and the icons) are [MIT](LICENSE) licensed. That license does not apply to the stories themselves.

## The book

For the history behind the reporting — London, money, and institutional power — see [The Veil, Vol I](https://www.theveilpress.com/library/veil/) (*The Veil of the Square Mile*). Softcover, hardcover, and digital editions are on the book page.

## Contact

Questions, or something you need that is not here: [deepdivefile@gmail.com](mailto:deepdivefile@gmail.com).
