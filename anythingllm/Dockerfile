# Dockerfile
ARG BUILD_FROM=ghcr.io/home-assistant/amd64-base:latest

FROM $BUILD_FROM

LABEL maintainer="Your Name <you@example.com>"

RUN apk add --no-cache \
    python3 py3-pip git ca-certificates && \
    update-ca-certificates

# Create a non‑root user
RUN adduser -D anythingllm

WORKDIR /opt/anythingllm

# 1️⃣ Clone the repo first (so we have the source)
RUN git clone https://github.com/Mintplex-Labs/anything-llm.git . \
    && rm -rf .git   # keep image small

# 2️⃣ Create & activate a virtualenv, then install deps
RUN python3 -m venv /opt/anythingllm/.venv && \
    . /.venv/bin/activate && \
    pip install --no-cache-dir -r requirements.txt

# 3️⃣ Set ownership and switch to the unprivileged user
RUN chown -R anythingllm:anythingllm /opt/anythingllm
USER anythingllm

EXPOSE 8501

# 4️⃣ Start the app inside the venv
ENTRYPOINT ["/bin/sh", "-c", ". /.venv/bin/activate && python -m streamlit run app.py --server.port=8501"]