# Day-6

Nesta etapa do treinamento, o estudo é focado em **volumes e armazenamento no Kubernetes**, abordando os principais objetos responsáveis por prover persistência de dados para aplicações.

Para melhor entendimento consultar a doc oficial do k8s:
- https://kubernetes.io/docs/concepts/storage/storage-classes/
- https://kubernetes.io/docs/concepts/storage/persistent-volumes/

## StorageClass

O **StorageClass** é um objeto do Kubernetes que descreve e define diferentes **classes de armazenamento** disponíveis no cluster.  
Ele permite abstrair os detalhes de provisionamento de storage, possibilitando a criação dinâmica de volumes de acordo com o tipo de armazenamento configurado.

Com o uso de StorageClasses, o administrador pode oferecer diferentes níveis de performance, custo e características de armazenamento para as aplicações.

## Persistent Volume (PV)

O **Persistent Volume (PV)** é um objeto que representa um recurso de armazenamento físico ou lógico disponível no cluster Kubernetes.  
Ele é provisionado pelo administrador do cluster ou dinamicamente por meio de um StorageClass.

Um PV pode representar:

- Um disco local em um Node
- Um dispositivo de armazenamento em rede
- Um serviço de armazenamento em nuvem

Existem diversos tipos e subtipos de armazenamento, entre eles:

- **Armazenamento local**
  - `hostPath`
- **Armazenamento em rede**
  - NFS (Network File System)
  - iSCSI (Internet Small Computer System Interface)
  - Ceph RBD (RADOS Block Device)
  - GlusterFS
- **Serviços de armazenamento em nuvem**
  - Volumes gerenciados por provedores cloud


## Persistent Volume Claim (PVC)

O **Persistent Volume Claim (PVC)** é uma solicitação de armazenamento feita por usuários ou aplicações dentro do cluster Kubernetes.  
Por meio do PVC, é possível solicitar um volume com base em critérios como **tamanho**, **modo de acesso** e **classe de armazenamento**.

O PVC atua como uma “requisição” que associa um **Persistent Volume** disponível a um Pod, permitindo que o container utilize o armazenamento de forma persistente, independentemente do ciclo de vida do Pod.


## Comandos

    kubectl get storageclasses
    kubectl get persistentvolume
    kubectl get persistentvolumeclaim
    kubectl get pods
    
    kubectl describe storageclasses
    kubectl describe persistentvolume {nome-do-pv}
    kubectl describe persistentvolumeclaim {nome-do-pvc}
    kubectl describe pods {nome-do-pod}

    kubectl apply -f storageclasse.yaml
    kubectl apply -f persistentvolume.yaml
    kubectl apply -f pv-nfs.yaml
    kubectl apply -f persistentvolumeclaim.yaml
    kubectl apply -f deployment.yaml

Comandos para instalacao do nfs server:

    sudo apt install nfs-kernel-server nfs-common # server e client
    mkdir /mnt/nfs # criar o diretorio nfs
    sudo echo "/mnt/nfs *(rw,sync,no_root_squash,no_subtree_check)" >> /etc/exports
    sudo exportfs -ar # atualiza o diretorio para compartilhamento
    showmount -e # visualiza os diretorios compartilhados atraves do nfs