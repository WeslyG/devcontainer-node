### Nodejs workspace

Версии тут [https://hub.docker.com/r/weslyg/nodejs-infra/tags](https://hub.docker.com/r/weslyg/nodejs-infra/tags)

Подключать вот так

```bash
touch .devcontainer
cat > .devcontainer/devcontainer.json <<EOF
{
    "name": "node22",
    "image": "weslyg/nodejs-infra:IMAGE_TAG"
}
EOF
```

Например можно сделать так

```bash
touch .devcontainer
cat > .devcontainer/devcontainer.json <<EOF
{
    "name": "NodeJs 22",
    "image": "weslyg/nodejs-infra:node22-bookworm-0225",
    "runArgs": [
        "--cap-add=SYS_ADMIN", // Optional: Adds capabilities required by some applications
        "--net=host" // Optional: Удобно, ходить в kubernetes и другие службы на локальном компе
    ],
    "mounts": [
        "source=${localEnv:HOME}${localEnv:USERPROFILE}/.ssh/,target=/home/node/.ssh,readonly,type=bind",
        "source=${localEnv:HOME}${localEnv:USERPROFILE}/.dotfiles,target=/home/node/.dotfiles,type=bind",
        "source=/var/run/docker.sock,target=/var/run/docker.sock,type=bind"
    ],
    "initializeCommand": "mkdir -p ~/.dotfiles",
    "postCreateCommand": "stow --dir=$HOME/.dotfiles --target=$HOME . --adopt  && sudo chown node:docker /var/run/docker.sock && yarn"
}
EOF
```