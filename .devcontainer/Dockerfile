FROM debian:bullseye-slim

ARG USERNAME=thomas
ARG USER_UID=1000
ARG USER_GID=${USER_UID}

# Remove any existing user/group with this UID/GID
RUN if id -u ${USER_UID} >/dev/null 2>&1; then userdel -f $(id -un ${USER_UID}) || true; fi

# Install base packages
RUN apt-get update && apt-get install -y --no-install-recommends \
    sudo curl ca-certificates git build-essential gnupg lsb-release \
    && rm -rf /var/lib/apt/lists/*

# Add non-root user with passwordless sudo
RUN groupadd --gid ${USER_GID} ${USERNAME} \
    && useradd --uid ${USER_UID} --gid ${USER_GID} -m ${USERNAME} \
    && echo "${USERNAME} ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/${USERNAME} \
    && chmod 0440 /etc/sudoers.d/${USERNAME}

# Install Node.js (LTS) and pnpm
RUN curl -fsSL https://deb.nodesource.com/setup_lts.x | bash - \
    && apt-get install -y nodejs \
    && npm install -g pnpm \
    && pnpm config set store-dir /home/${USERNAME}/.pnpm-store

# Create workspace and give ownership
RUN mkdir -p /workspace && chown ${USERNAME}:${USERNAME} /workspace

# Set default shell and working directory
ENV SHELL=/bin/bash
WORKDIR /workspace

# Use root by default for devcontainers
USER root

CMD ["/bin/bash"]
