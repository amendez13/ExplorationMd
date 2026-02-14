[← Back to Topic](README.md) | [← Home](../../README.md)

# MCP Servers Catalog: Exa and Ref

## Index

- [Why These Two](#why-these-two)
- [Exa MCP Server](#exa-mcp-server)
- [Ref MCP Server](#ref-mcp-server)
- [Common Setup Patterns](#common-setup-patterns)
- [Operational Notes](#operational-notes)
- [Sources](#sources)

---

## Why These Two

An X post called out two “favorite” MCP servers that “have not changed for months”: **Exa** and **Ref**. [1]

In practice they cover two common “external knowledge” needs for coding agents:
- **Search / research:** find relevant web pages, code examples, people, and company info (Exa). [2] [3]
- **Documentation retrieval:** search and read API/library docs with a bias toward token efficiency (Ref). [4] [5]

## Exa MCP Server

Exa MCP connects an assistant to Exa’s hosted MCP endpoint (`https://mcp.exa.ai/mcp`) for web and code search plus related “research” workflows. [2]

Notable capabilities (as documented by the server README):
- **Web search and “advanced” web search:** including filters and content options (via `web_search_exa` and `web_search_advanced_exa`). [2]
- **Code context search:** find code examples and programming solutions from sources like GitHub/Stack Overflow/docs (`get_code_context_exa`). [2]
- **Company and people research:** `company_research_exa` and `people_search_exa`. [2]
- **Asynchronous deep research:** start a research task and later check results (`deep_researcher_start` / `deep_researcher_check`). [2]

## Ref MCP Server

Ref MCP is positioned as a documentation-focused MCP server: it provides tools for searching documentation and reading relevant sections of docs pages, aiming to keep results “fast and token-efficient.” [4] [5]

Key ideas highlighted in the project README:
- **Session-aware retrieval:** uses MCP sessions to track “search trajectory” and reduce repeated results. [4]
- **Targeted reading:** attempts to return only the relevant part of a documentation page (described as ~5k tokens). [4]
- **Two core tools:** `ref_search_documentation` and `ref_read_url`. [4]

## Common Setup Patterns

High-level configuration shapes described in the projects’ documentation:
- **Hosted HTTP MCP endpoints:** Exa documents `https://mcp.exa.ai/mcp` as a hosted MCP server. [2]
- **Ref hosted endpoint:** Ref documents `https://api.ref.tools/mcp?apiKey=YOUR_API_KEY` for an HTTP setup. [4]
- **Local stdio via `npx`:** both projects document `npx`-based local setup flows (Exa: `exa-mcp-server`; Ref: `ref-tools-mcp@latest`). [2] [4]

## Operational Notes

- **API keys:** both Exa and Ref are documented as requiring API keys for typical use. [2] [4]
- **Context management remains your problem:** Ref emphasizes token minimization to reduce “context rot”; Exa’s README includes “token isolation” guidance in its Claude skills. [2] [4]
- **Ecosystem placement:** Exa is listed in the community server list in the MCP servers repository. [3]

## Sources

1. X post (excerpt provided by user) (note://78)
2. [github.com/exa-labs/exa-mcp-server](https://github.com/exa-labs/exa-mcp-server)
3. [github.com/modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers)
4. [github.com/ref-tools/ref-tools-mcp](https://github.com/ref-tools/ref-tools-mcp)
5. [ref.tools](https://ref.tools/)

