# Dockerfile
ARG BUILD_FROM=ghcr.io/home-assistant/amd64-base:latest

FROM $BUILD_FROM

LABEL maintainer="Your Name <you@example.com>"

# Install system packages that the base image needs
RUN apk add --no-cache \
    python3 py3-pip git ca-certificates && \
    update-ca-certificates

WORKDIR /opt/anythingllm

# 1️⃣ Clone the Anything‑LLM repo (remove .git to keep image small)
RUN git clone https://github.com/Mintplex-Labs/anything-llm.git . && rm -rf .git

# 2️⃣ Create a virtual environment
RUN python3 -m venv /opt/anythingllm/.venv

# 3️⃣ Activate the venv and install the repo as an editable package
RUN . /opt/anythingllm/.venv/bin/activate && \
    pip install --no-cache-dir -e .

# 4️⃣ Give ownership to the unprivileged user that runs the container
RUN chown -R anythingllm:anythingllm /opt/anythingllm
USER anythingllm

EXPOSE 8501

# 5️⃣ Start Streamlit inside the venv
ENTRYPOINT ["/bin/sh", "-c",
            ". /opt/anythingllm/.venv/bin/activate && python -m streamlit run app.py --server.port=8501"]