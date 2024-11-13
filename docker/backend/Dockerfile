FROM python:3.11-slim as base

# Set environment variables
ENV PYTHONUNBUFFERED=1 \
    PYTHONDONTWRITEBYTECODE=1 \
    PIP_NO_CACHE_DIR=1 \
    PIP_DISABLE_PIP_VERSION_CHECK=1

WORKDIR /app

# Install system dependencies
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        build-essential \
        curl \
        git \
        libpq-dev \
    && rm -rf /var/lib/apt/lists/*

# Copy requirements files
COPY backend/requirements/base.txt backend/requirements/dev.txt ./requirements/

# Install Python dependencies
RUN pip install -r requirements/base.txt

# Development stage
FROM base as development

# Install development dependencies
RUN pip install -r requirements/dev.txt

# Create upload directory
RUN mkdir -p /app/uploads && chmod 777 /app/uploads

# Copy backend code only
COPY backend .

# Start development server
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000", "--reload"]

# Production stage
FROM base as production

# Create upload directory
RUN mkdir -p /app/uploads && chmod 777 /app/uploads

# Copy backend code only
COPY backend .

# Start production server
CMD ["gunicorn", "app.main:app", "-w", "4", "-k", "uvicorn.workers.UvicornWorker", "-b", "0.0.0.0:8000"]