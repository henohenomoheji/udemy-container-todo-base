FROM mcr.microsoft.com/devcontainers/base:ubuntu

# 必要最低限のツール
RUN apt-get update && apt-get install -y \
    ca-certificates curl unzip gnupg lsb-release \
    && rm -rf /var/lib/apt/lists/*

# Azure CLI
RUN curl -sL https://aka.ms/InstallAzureCLIDeb | bash

# azd (Azure Developer CLI)
RUN curl -fsSL https://aka.ms/install-azd.sh | bash

# Bicep CLI（az bicep でも良いが、単体bicepも入れておくと安定）
RUN az bicep install

# 便利：補完（任意）
RUN az completion -h >/dev/null 2>&1 || true
