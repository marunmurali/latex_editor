FROM ubuntu:24.04

ENV DEBIAN_FRONTEND=noninteractive \
    LANG=C.UTF-8

# Full TeX Live (every scheme/collection) + build driver + basic dev tooling.
RUN apt-get update && apt-get install -y --no-install-recommends \
        texlive-full \
        latexmk \
        git \
        curl \
        ca-certificates \
        sudo \
        gnupg \
    && rm -rf /var/lib/apt/lists/*

# Claude Code CLI requires Node >=22; Ubuntu 24.04's apt package is 18.x, so
# pull Node 22 from NodeSource (public repo) instead.
RUN curl -fsSL https://deb.nodesource.com/setup_22.x | bash - \
    && apt-get install -y --no-install-recommends nodejs \
    && rm -rf /var/lib/apt/lists/*

# Claude Code CLI (powers the "Claude Code" VS Code extension in this container).
RUN npm install -g @anthropic-ai/claude-code

# ubuntu:24.04 already ships a uid/gid-1000 "ubuntu" user; just grant it
# passwordless sudo instead of creating a new one (avoids a UID/GID clash).
RUN echo "ubuntu ALL=(root) NOPASSWD:ALL" > /etc/sudoers.d/ubuntu \
    && chmod 0440 /etc/sudoers.d/ubuntu

USER ubuntu
