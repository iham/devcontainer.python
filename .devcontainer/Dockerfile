# syntax=docker/dockerfile:1
ARG PYTHON="3.11"
ARG DEBIAN="bookworm"

FROM python:${PYTHON}-${DEBIAN} AS base

RUN  apt-get update \
  && apt-get install -y sudo git curl wget zsh \
  && rm -rf /var/lib/apt/lists/*

# install python stuff
RUN pip install plonecli

# adding plone user
ARG USER_NAME="plone"
ARG USER_UID=1000
ARG USER_GID=$USER_UID
RUN <<EOT
  groupadd --gid ${USER_GID} plone
  useradd --uid ${USER_UID} --gid ${USER_GID} --shell /bin/zsh -m ${USER_NAME}
EOT

USER plone
WORKDIR /home/${USER_NAME}

# install ohmyzsh with powerlevel10k
ENV ZSH_CUSTOM="/home/${USER_NAME}/.oh-my-zsh/custom"
RUN <<EOT
  sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
  git clone --depth 1 https://github.com/marlonrichert/zsh-autocomplete.git $ZSH_CUSTOM/plugins/zsh-autocomplete
  git clone https://github.com/zsh-users/zsh-autosuggestions.git $ZSH_CUSTOM/plugins/zsh-autosuggestions
  git clone https://github.com/zsh-users/zsh-completions $ZSH_CUSTOM/plugins/zsh-completions
  git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH_CUSTOM/plugins/zsh-syntax-highlighting
  git clone --depth 1 https://github.com/romkatv/powerlevel10k.git $ZSH_CUSTOM/themes/powerlevel10k
EOT
COPY .devcontainer/.p10k.zsh .devcontainer/.zshrc ./