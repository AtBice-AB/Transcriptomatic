# Builder Stage
FROM docker.io/nvidia/cuda:12.2.2-base-ubuntu22.04 AS builder

ENV DEBIAN_FRONTEND=noninteractive

# Install system dependencies, Python 3.11, and uv
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        software-properties-common \
        curl \
        build-essential \
        gcc && \
    add-apt-repository ppa:deadsnakes/ppa -y && \
    apt-get update && \
    apt-get install -y --no-install-recommends \
        python3.11 \
        python3.11-dev \
        python3.11-venv && \
    rm -rf /var/lib/apt/lists/* && \
    apt-get clean

COPY --from=ghcr.io/astral-sh/uv:0.1.39 /uv /usr/local/bin/uv
RUN uv --version

WORKDIR /app

# Create the virtual environment
RUN python3.11 -m venv /opt/venv

# --- FIX STARTS HERE ---
# Set environment variables so subsequent commands use the venv
ENV VIRTUAL_ENV=/opt/venv
ENV PATH="/opt/venv/bin:$PATH"
# --- FIX ENDS HERE ---

# Verify python is now from venv (optional sanity check)
RUN which python

COPY pyproject.toml .
# Compile requirements (uv compile doesn't strictly need activation but doesn't hurt)
RUN uv pip compile pyproject.toml -o requirements.txt
# Install requirements - uv install now finds the VIRTUAL_ENV
RUN uv pip install --no-cache-dir -r requirements.txt

# Final Stage
FROM docker.io/nvidia/cuda:12.2.2-runtime-ubuntu22.04

ENV DEBIAN_FRONTEND=noninteractive

# Install only necessary runtime dependencies
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        python3.11 \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get clean

WORKDIR /app

# Copy the virtual environment from the builder stage
COPY --from=builder /opt/venv /opt/venv
# Copy application code and .env file
COPY . .
COPY .env /app/.env

# Set the PATH globally for the final container runtime
# This ensures the CMD uses python/uvicorn from the venv
ENV PATH="/opt/venv/bin:$PATH"

EXPOSE 8000

# Use uvicorn from the virtual environment
CMD ["sh", "-c", "python download_models.py && uvicorn api:app --host 0.0.0.0 --port 8000"]