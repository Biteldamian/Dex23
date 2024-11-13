FROM python:3.9-slim

WORKDIR /app

RUN apt-get update && apt-get install -y \
    build-essential \
    python3-dev \
    curl \
    && rm -rf /var/lib/apt/lists/*

RUN pip install --no-cache-dir \
    chromadb==0.4.15 \
    uvicorn[standard]

EXPOSE 8000

CMD ["python", "-m", "uvicorn", "chromadb.app:app", "--host", "0.0.0.0", "--port", "8000"]