# Day-4

Para melhor entendimento consultar a doc oficial do k8s:
- https://kubernetes.io/docs/concepts/workloads/controllers/deployment/
- https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/
- https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/
- https://kubernetes.io/pt-br/docs/concepts/workloads/controllers/replicaset/

## Deployment
O Deployment gerencia o ciclo de vida das aplicações, garantindo atualizações controladas, rollback e o gerenciamento de réplicas por meio de ReplicaSets.

## DeamonSet
O DaemonSet é um objeto que garante que todos os Nodes do cluster executem uma réplica de um determinado Pod.
Ele é muito usado para agentes de monitoramento, coleta de logs e ferramentas de rede.

## Probes
As probes são mecanismos de verificação que permitem ao Kubernetes monitorar o estado interno de um container.
Elas garantem que seus Pods estejam funcionando corretamente e que o Kubernetes saiba quando reiniciar, iniciar ou parar o tráfego para um container.

### Liveness
A livenessProbe verifica se o processo dentro do Pod está saudável.
Se a verificação falhar, o Kubernetes reinicia o container.

### Readiness
A readinessProbe verifica se o container está pronto para receber tráfego.
Enquanto ela falhar, o Kubernetes remove o Pod do Service, impedindo que receba requisições.

### StartUp
A startupProbe verifica se o container foi inicializado corretamente e está pronto para começar a operar.
Ela só é executada durante a fase de inicialização do Pod. Depois de aprovada, não roda novamente.

## ReplicaSet
O ReplicaSet é criado pelo Deployment e tem a função de gerenciar os Pods e suas réplicas.
Sua criação ocorre automaticamente pelo Deployment e, a cada atualização, um novo ReplicaSet é gerado.
Dessa forma, mantém-se um histórico, permitindo realizar rollback caso seja necessário.


## Comandos

    kubectl apply -f namespace.yaml
    kubectl apply -f nginx-deployment
    
    kubectl describe replicasets -n day-4 {nome-replicaset}
    kubectl describe pods -n day-4 {nome-pod}
    ubectl describe daemonsets -n day-4
    
    kubectl rollout undo deployment nginx-deployment
    
    kubectl scale deployment -n day-4 nginx-deployment --replicas 5

    ubectl get daemonsets -n day-4
    kubectl get deployment -n day-4
    kubectl get replicasets -n day-4
    kubectl get pods -n day-4
    
    kubectl get pods -o=jsonpath='{range .items[*]}{"\n"}{.metadata.name}{"\t"}{range .spec.containers[*]}{.image}{"\t"}{end}{end}'

-o=jsonpath: esse parâmetro especifica que queremos usar a saída em formato JSONPath para exibir as informações dos Pods.

'{range .items[*]}{"\n"}{.metadata.name}{"\t"}{range .spec.containers[*]}{.image}{"\t"}{end}{end}': essa é a expressão JSONPath que define o formato de saída do comando. Ela usa a função range para iterar sobre todos os objetos items (ou seja, os Pods) retornados pelo comando kubectl get pods. Em seguida, exibe o nome do Pod ({.metadata.name}) seguido de uma tabulação (\t), e itera sobre todos os contêineres ({range .spec.containers[*]}) dentro do Pod, exibindo a imagem usada por cada um deles ({.image}). Por fim, insere uma quebra de linha (\n) e fecha o segundo range com {end}{end}.




