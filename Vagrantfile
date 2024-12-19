Vagrant.configure("2") do |config|
  # Configuration commune pour toutes les machines virtuelles
  # Utilisation d'Ubuntu 20.04 (Focal)
  config.vm.box = "ubuntu/focal64"

  # Si vous avez le plugin vagrant-disksize, vous pouvez spécifier la taille du disque :
  # config.disksize.size = "20GB"

  # config.vm.network "private_network", type: "dhcp"


  config.vm.provider "virtualbox" do |vb|
    vb.memory = "8192" # 2 Go de RAM
    vb.cpus = 4        # 2 CPU
  end

  # Configuration de la machine virtuelle Master Node
  config.vm.define "master-node" do |master|
    master.vm.hostname = "master-node"
    master.vm.network "private_network", ip: "192.168.56.101"

    master.vm.provision "shell", inline: <<-SHELL
      # Désactivation du swap
      sudo swapoff -a

      # Configuration du forwarding IP
      sudo modprobe overlay
      sudo modprobe br_netfilter
      echo "net.bridge.bridge-nf-call-iptables=1" | sudo tee /etc/sysctl.d/k8s.conf
      echo "net.bridge.bridge-nf-call-ip6tables=1" | sudo tee /etc/sysctl.d/k8s.conf
      echo "net.ipv4.ip_forward=1" | sudo tee /etc/sysctl.d/k8s.conf

      sudo sysctl --system

      lsmod | grep br_netfilter
      lsmod | frep overlay

      # Installation de containerd
      sudo apt-get update
      sudo apt-get install -y ca-certificates curl
      sudo install -m 0755 -d /etc/apt/keyrings
      sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
      sudo chmod a+r /etc/apt/keyrings/docker.asc
      echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo \"$VERSION_CODENAME\") stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
      sudo apt-get update
      sudo apt-get install -y containerd.io
      sudo mkdir -p /etc/containerd
      containerd config default | sed 's/SystemdCgroup = false/SystemdCgroup = true/' | sed 's/sandbox image = "registry.k8s.io\\/pause:3.6"/sandbox image = "registry.k8s.io\\/pause:3.9"/' | sudo tee /etc/containerd/config.toml
      sudo systemctl restart containerd

      # Installation de kubeadm, kubelet, kubectl
      sudo apt-get install -y apt-transport-https ca-certificates curl gpg
      curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.31/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
      echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.31/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
      sudo apt-get update
      sudo apt-get install -y kubelet kubeadm kubectl
      sudo apt-mark hold kubelet kubeadm kubectl
    SHELL
  end

  # Configuration de la machine virtuelle Worker Node 1
  config.vm.define "worker-node-1" do |worker1|
    worker1.vm.hostname = "worker-node-1"
    worker1.vm.network "private_network", ip: "192.168.56.102"
    worker1.vm.provision "shell", inline: <<-SHELL
      sudo swapoff -a

      # Configuration du forwarding IP
      sudo modprobe overlay
      sudo modprobe br_netfilter
      echo "net.bridge.bridge-nf-call-iptables=1" | sudo tee /etc/sysctl.d/k8s.conf
      echo "net.bridge.bridge-nf-call-ip6tables=1" | sudo tee /etc/sysctl.d/k8s.conf
      echo "net.ipv4.ip_forward=1" | sudo tee /etc/sysctl.d/k8s.conf

      sudo sysctl --system

      lsmod | grep br_netfilter
      lsmod | frep overlay

      sudo apt-get update
      sudo apt-get install -y ca-certificates curl
      sudo install -m 0755 -d /etc/apt/keyrings
      sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
      sudo chmod a+r /etc/apt/keyrings/docker.asc
      echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo \"$VERSION_CODENAME\") stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
      sudo apt-get update
      sudo apt-get install -y containerd.io
      sudo mkdir -p /etc/containerd
      containerd config default | sed 's/SystemdCgroup = false/SystemdCgroup = true/' | sed 's/sandbox image = "registry.k8s.io\\/pause:3.6"/sandbox image = "registry.k8s.io\\/pause:3.9"/' | sudo tee /etc/containerd/config.toml
      sudo systemctl restart containerd

      sudo apt-get install -y apt-transport-https ca-certificates curl gpg
      curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.31/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
      echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.31/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
      sudo apt-get update
      sudo apt-get install -y kubelet kubeadm kubectl
      sudo apt-mark hold kubelet kubeadm kubectl
    SHELL
  end

  # Configuration de la machine virtuelle Worker Node 2
  config.vm.define "worker-node-2" do |worker2|
    worker2.vm.hostname = "worker-node-2"
    worker2.vm.network "private_network", ip: "192.168.56.103"
    worker2.vm.provision "shell", inline: <<-SHELL
      sudo swapoff -a

      # Configuration du forwarding IP
      sudo modprobe overlay
      sudo modprobe br_netfilter
      echo "net.bridge.bridge-nf-call-iptables=1" | sudo tee /etc/sysctl.d/k8s.conf
      echo "net.bridge.bridge-nf-call-ip6tables=1" | sudo tee /etc/sysctl.d/k8s.conf
      echo "net.ipv4.ip_forward=1" | sudo tee /etc/sysctl.d/k8s.conf

      sudo sysctl --system

      lsmod | grep br_netfilter
      lsmod | frep overlay

      sudo apt-get update
      sudo apt-get install -y ca-certificates curl
      sudo install -m 0755 -d /etc/apt/keyrings
      sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
      sudo chmod a+r /etc/apt/keyrings/docker.asc
      echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo \"$VERSION_CODENAME\") stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
      sudo apt-get update
      sudo apt-get install -y containerd.io
      sudo mkdir -p /etc/containerd
      containerd config default | sed 's/SystemdCgroup = false/SystemdCgroup = true/' | sed 's/sandbox image = "registry.k8s.io\\/pause:3.6"/sandbox image = "registry.k8s.io\\/pause:3.9"/' | sudo tee /etc/containerd/config.toml
      sudo systemctl restart containerd

      sudo apt-get install -y apt-transport-https ca-certificates curl gpg
      curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.31/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
      echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.31/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
      sudo apt-get update
      sudo apt-get install -y kubelet kubeadm kubectl
      sudo apt-mark hold kubelet kubeadm kubectl
    SHELL
  end
end
