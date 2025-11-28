# Day-2
Nesta parte do treinamento, o foco está no entendimento mais profundo sobre Pods.
Aqui ficam alguns exemplos de Pods, e abaixo estão os comandos utilizados nesta etapa do treinamento.

## Manifesto yaml
O arquivo pod.yaml busca incluir a maioria dos campos estudados nesta etapa do treinamento.
No diretório exemplo/ estão os recursos e exemplos utilizados durante as leituras e práticas.

## Comandos
Comando usados no day-2:

    kubectl get nodes #exibe os nodes
    kubectl get ns #exibe os namespaces
    
    kubectl get pods --help #exibe comandos envolvendo Pods
    kubectl get pods #exibe os Pods no namespace default
    kubectl get pods -A #kubectl get pods --all-namespacs exibe todos os Pods em todos os namespaces
    kubectl get pods -n namespace #exibe os Pods do namespace especifico
    kubectl get pods -w #exibe os Pods e mantem o processo sendo escutado
    kubectl get pods nome-do-pod -o yaml #exibe a descricao do Pod em um manifesto yaml
    kubectl get pods -o wide #exibe mais informacoes como IP, NODE
    
    kubectl describe pods nome-do-pod #exibe informacoes detalhadas sobre o Pod
    
    kubectl apply -f pod.yaml #aplica o manifesto
    kubectl delete pods nome-do-pod #deleta o Pod
    kubectl delete -f pod.yaml #deleta o Pod referente ao manifesto
    
    kubectl logs nome-do-pod #consulta os logs do Pod
    kubectl logs nome-do-pod -c nome-do-container #consulta os logs do container dentro do pod
    
    kubectl run nome-do-pod --image nome-da-imagem --port valor-da-porta #cria um pod e executa a imagem dentro do Pod configurando uma porta
    kubectl run nome-do-pod --image nome-da-imagem --port valor-da-porta -n namespace #cria um pod e executa a imagem dentro do Pod configurando uma porta e configura um namespace

    kubectl run nome-do-pod --image nome-da-imagem --dry-run=client -o yaml > pod.yaml #faz a criação fake do pod para obter um manifesto em formato yaml e envia a saida do comando para um arquivo de texto (neste caso ja com a nome pod.yaml)
    
    kubectl exec -it nome-do-pod -c nome-do-container -- comando-linux #comando para executar comandos dentro do Pod (na maioria das vezes sera um comando GNU/Linux como: bash, sh, ls e etc)