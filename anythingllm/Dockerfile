# Dockerfile
ARG BUILD_FROM=ghcr.io/home-assistant/amd64-base:latest

FROM $BUILD_FROM

LABEL maintainer="Your Name <you@example.com>"

RUN apk add --no-cache \
    python3 py3-pip git ca-certificates && \
    update-ca-certificates

WORKDIR /opt/anythingllm
RUN git clone https://github.com/Mintplex-Labs/anything-llm.git . && rm -rf .git

# 1️⃣ Create virtual environment
RUN python3 -m venv /opt/anythingllm/.venv

# 2️⃣ Activate it and install deps
RUN . /opt/anythingllm/.venv/bin/activate && \
    pip install --no-cache-dir -r requirements.txt

# Make sure the unprivileged user owns everything
RUN chown -R anythingllm:anythingllm /opt/anythingllm
USER anythingllm

EXPOSE 8501

ENTRYPOINT ["/bin/sh", "-c", ". /opt/anythingllm/.venv/bin/activate && python -m streamlit run app.py --server.port=8501"]