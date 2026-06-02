FROM python:3.11-slim

WORKDIR /workspace

RUN apt-get update && apt-get install -y \
    ffmpeg \
    imagemagick \
    curl \
    && rm -rf /var/lib/apt/lists/*

# Create non-root user with proper permissions
RUN useradd -m -u 1000 appuser && \
    mkdir -p /home/appuser && \
    chown -R appuser:appuser /home/appuser

# Install API dependencies
COPY apps/api/requirements.txt /workspace/apps/api/requirements.txt
RUN pip install --no-cache-dir -r /workspace/apps/api/requirements.txt

# Copy transcoder package and API
COPY packages/transcoder /workspace/packages/transcoder
COPY apps/api /workspace/apps/api

# Set proper permissions for workspace
RUN chown -R appuser:appuser /workspace

WORKDIR /workspace

# Switch to non-root user
USER appuser

CMD ["uvicorn", "apps.api.main:app", "--host", "0.0.0.0", "--port", "8000"]
