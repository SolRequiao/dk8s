# Day-5

Nesta etapa do treinamento foi configurado um **cluster Kubernetes**.  
Optei por utilizar o **VirtualBox** com **Ubuntu Server** para a criação do ambiente.

---

## Cluster

O cluster foi composto por:

- **1 VM Control Plane**
  - 4 GB de RAM
  - 2 CPUs
  - 5 GB de armazenamento

- **2 VMs Worker**
  - 2 GB de RAM
  - 2 CPUs
  - 5 GB de armazenamento

---

## Portas abertas

Foi necessário permitir, no **firewall**, o acesso às seguintes portas:

- TCP 6443
- TCP 10250–10255
- TCP 30000–32767
- TCP 2379–2380
- TCP 6783
- UDP 6783
- UDP 6784

---

## Configuração

Vale lembrar que, por se tratar de máquinas virtuais, caso alguma configuração não funcione corretamente, **às vezes é necessário reiniciar as VMs**.

---

### Em todas as VMs

Lembrar de desativar o swap:

    sudo swapoff -a


Modulos do kernel

    sudo vim /etc/modules-load.d/k8s.conf
    overlay
    br_netfilter
    sudo modprobe overlay
    sudo modprobe br_netfilter

Parametros do sistema:

    sudo vim /etc/sysctl.d/k8s.conf
    net.bridge.bridge-nf-call-iptables = 1
    net.bridge.bridge-nf-call-ip6tables = 1
    net.ipv4.ip_forward = 1
    sudo sysctl --system

Instalando os pacotes:

    sudo apt-get update
    # apt-transport-https may be a dummy package; if so, you can skip that package
    sudo apt-get install -y apt-transport-https ca-certificates curl gpg
    # If the directory `/etc/apt/keyrings` does not exist, it should be created before the curl command, read the note below.
    # sudo mkdir -p -m 755 /etc/apt/keyrings
    curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.35/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
    # This overwrites any existing configuration in /etc/apt/sources.list.d/kubernetes.list
    echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.35/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
    sudo apt-get update
    sudo apt-get install -y kubelet kubeadm kubectl
    sudo apt-mark hold kubelet kubeadm kubectl
    sudo systemctl enable --now kubelet
    
    

Instalando o containerd:

    sudo apt-get update && sudo apt-get install -y apt-transport-https ca-certificates curl gnupg lsb-release

    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

    echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

    sudo apt-get update && sudo apt-get install -y containerd.io

Configurando o containerd:

    sudo containerd config default | sudo tee /etc/containerd/config.toml

    sudo sed -i 's/SystemdCgroup = false/SystemdCgroup = true/g' /etc/containerd/config.toml

    sudo systemctl restart containerd
    sudo systemctl status containerd

Habilitando o serviço do kubelet:

    sudo systemctl enable --now kubelet

### Comando no Control Plane

    sudo kubeadm init --pod-network-cidr=10.10.0.0/16 --apiserver-advertise-address=<IP-do-control-plane>
    mkdir -p $HOME/.kube
    sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
    sudo chown $(id -u):$(id -g) $HOME/.kube/config


### Comando no Worker

Adicionando nodes worker ao cluster:

    sudo kubeadm join IP:PORTA --token VALOR-DO-TOKEN --discovery-token-ca-cert-hash VALOR-DA-HASH

Caso você não tenha anotado o comando acima, execute o comando abaixo no Control Plane:

    sudo kubeadm token create --print-join-command

### Volte ao Control Plane

Neste primeiro comandoa, o status dos nodes estaram NotReady:
    
    kubectl get nodes

Instalando o Weave Net:

    kubectl apply -f https://github.com/weaveworks/weave/releases/download/v2.8.1/weave-daemonset-k8s.yaml

Esper alguns instantes para que todos os componentes estejam funcionando corretamente:

    watch kubectl get nodes

Apos todos os nodes estarem com o status = Ready, crie um dployment simples para testar (com mais de um pod):

    kubectl create deployment nginx --image=nginx --replicas 3
    watch kubectl get pods -o wide

