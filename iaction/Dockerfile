ARG BUILD_FROM=docker.io/library/python:3.12
FROM ${BUILD_FROM}

LABEL org.opencontainers.image.source="https://github.com/LucasSch1/IAction-ha-addon-multi-cam" \
      org.opencontainers.image.title="IAction (Home Assistant add-on)" \
      org.opencontainers.image.description="Image analysis with AI + MQTT, web UI, and Home Assistant integration" \
      org.opencontainers.image.licenses="MIT"

# Install dependencies
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y --no-install-recommends \
    git \
    python3-opencv \
    ffmpeg \
  && rm -rf /var/lib/apt/lists/*

# Create app directory & fetch source code
WORKDIR /app
RUN apt-get update && apt-get install -y --no-install-recommends curl unzip \
 && curl -fL https://codeload.github.com/LucasSch1/IAction-ha-addon-multi-cam/zip/refs/heads/main -o iaction.zip
 && unzip iaction.zip \
 && mv IAction-main/* . \
 && rm -rf iaction.zip IAction-main \
 && rm -rf /var/lib/apt/lists/*


# Create and populate virtual environment
RUN python -m venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"
RUN pip install --upgrade pip
RUN pip install --no-cache-dir -r requirements.txt

# Copy entrypoint script
COPY entrypoint.sh /app/entrypoint.sh
RUN chmod +x /app/entrypoint.sh

# Start application
ENTRYPOINT ["/app/entrypoint.sh"]
