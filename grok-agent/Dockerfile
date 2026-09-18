FROM python:3.12-slim

ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    PIP_NO_CACHE_DIR=1

RUN pip install --no-cache-dir "scrapling[all]>=0.4.15" \
    && scrapling install --force

EXPOSE 8000

CMD ["sh", "-c", "exec scrapling-mcp --http --host 0.0.0.0 --port 8000"]
