# project-kubernetes

## ROADMAP 

Chaque service correspond à un container. Il faut un déploiement par container.

curl -fsSL https://packages.cloud.google.com/apt/doc/apt-key.gpg

sudo tee /etc/apt/trusted.gpg.d/kubernetes-archive-keyring.gpg > /dev/null

echo "deb [signed-by=/etc/apt/trusted.gpg.d/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" 

sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt-get update


