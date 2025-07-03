# 🧰 Model Context Protocol (MCP) Integration with Mi:dm 2.0

This guide shows how to enable **tool-calling** in Mi:dm 2.0 using [MCP Support of Open WebUI](https://docs.openwebui.com/openapi-servers/mcp).

---

## 📁 File Structure

To get started, download the following files and place them in the same structure as this repository:

```
.
├── docker-compose.yml
└── mcp/
    ├── Dockerfile
    └── mcp.json
```

- [`docker-compose.yml`](./docker-compose.yml)
- [`mcp/Dockerfile`](./mcp/Dockerfile)
- [`mcp/mcp.json`](./mcp/mcp.json)

<br>

## 🚀 1. Run Open WebUI with MCP Server

With the files in place, launch both Open WebU and the MCP server using Docker Compose:

```bash
docker compose up -d --build
```

Once the containers are running, you can access the services at:
* Open WebUI: http://localhost:3000/ or http://127.0.0.1:3000/
* MCP tools (OpenAPI UI): http://localhost:8000/docs

> **📝 Note:**  
> Ensure that ports `3000` and `8000` are available and not blocked by other applications or firewalls.

You can connect Mi:dm 2.0 via Ollama as described in the [previous guide](../README.md#-2-connect-midm-via-ollama).

<br>

## 🔗 2. Register Tools in Open WebUI

To enable tool usage in Mi:dm 2.0, add the following endpoints in Open WebUI:

1. Click the **profile icon** in the top-right corner.

2. Go to **Settings** → **Tools**.

3. Add the following tool URLs:
* http://localhost:8000/server-time
* http://localhost:8000/ddg-search

Once added, these tools will appear as available functions within the chat interface.

<br>

## 💬 3. Try Tool-Calling in Chat

Now you can test tool-based interactions with natural prompts.
Here are some examples you can try:

* `지금 서울 몇시야?`
* `2025년 최저임금 알려줘`
* `"AI Trends" 검색해서 상위 3개 링크 내용 알려줘`
* `다음 링크의 주요 내용을 요약해줘: https://www.yna.co.kr/view/AKR20250703034100017?input=1195m`

Mi:dm 2.0 will automatically choose the appropriate tool and return a dynamic response using function calling.

<br>

## 🛠️ 4. Customize and Extend

You can add more tools by editing:

* `mcp/Dockerfile` — to install and launch new tool servers
* `mcp/mcp.json` — to define command and route mappings for additional tools

> **📝 Note:**  
> Rebuild your container (`docker compose build && docker compose up -d --build`) after making changes.

---

🐾 *Take full advantage of Mi:dm 2.0’s function calling capabilities with seamless MCP integration in Open WebUI!*

