# Kind
A configuração do cluster Kind está dentro do diretório kind/. Para criar o cluster com essas configurações, após o arquivo YAML estar criado e configurado, utilize o comando:

    kind create cluster --config cluster-kind.yaml  --name {nome-do-cluster}

Caso o arquivo esteja em outro diretório, a opção --config deve incluir também o caminho completo até o arquivo de configuração do Kind.

# Pod
O Pod foi criado usando um comando muito prático:

    kubectl run --image nginx --port 80 nginx-giropops --dry-run=client -o yaml > pod.yaml

Explicando por etapas:

### 1 - kubectl run --image {nome-da-imagem} --port {valor-da-porta} {nome-do-pod}
Esse comando cria o Pod usando a imagem informada, define a porta de comunicação com o container e atribui um nome ao Pod.

### --dry-run=client -o yaml
Essa opção faz com que o kubectl simule a criação do Pod e imprima na tela o template YAML correspondente, sem efetivamente criar o recurso.

### > pod.yaml
Direciona a saída do YAML gerado para um arquivo chamado pod.yaml.

## Rodando o pod
Para realmente subir o Pod, é necessário usar o comando:

    kubectl apply -f pod.yaml