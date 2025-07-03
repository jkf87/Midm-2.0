FROM python:3.11-slim

WORKDIR /app

RUN pip install --no-cache-dir uv mcpo

COPY mcp.json .

CMD ["uvx", "mcpo", "--host", "0.0.0.0", "--port", "8000", "--config", "mcp.json"]
