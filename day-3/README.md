# Day-3
Nesta parte do treinamento, o foco está no entendimento geral sobre Deployments.

Os manifestos buscam incluir a maioria dos campos estudados nesta etapa do treinamento.

## Comandos
Comando usados no day-3:

    kubectl get deployments
    kubectl get deployments -o yaml
    kubectl get pods
    kubectl get pods -l app=nginx-deployment #filtrando os pods pela label
    kubectl get replicasets
   
    kubectl create deployment --image nginx --replicas 3 nginx-deployment --dry-run=client -o yaml > deployment.yaml
    kubectl create namespace day-3 --dry-run=client -o yaml > namespace.yaml
   
    kubectl apply -f deployment.yaml
    kubectl apply -f namespace.yaml

    kubectl delete -f deployment.yaml
    kubectl delete -f namespace.yaml

    kubectl rollout status deployment -n day-3 nginx-deployment #mostra o status do deployment (neste caso)
    
    kubectl rollout history deployment -n day-3 nginx-deployment #historico de revision
    kubectl rollout history deployment -n day-3 nginx-deployment --revision {valor-revision} #verifica o conteudo das revision
    kubectl rollout history deployment -n day-3 nginx-deployment --revision #voltando pra uma revision especifica
    
    kubectl rollout undo deployment -n day-3 nginx-deployment #faz o rollback pra a penultima revision
    kubectl rollout undo deployment -n day-3 nginx-deployment --to-revision 2 #volta para uma revision especifica

    kubectl rollout pause deployment -n day-3 nginx-deployment #para o deployment (pode evitar apply em aplicacoes criticas, pode ser usado como medidad de segurança)
    kubectl rollout resume deployment -n day-3 nginx-deployment #volta a executar (tira o pause)
    kubectl rollout restart deployment -n day-3 nginx-deployment #restarta todo os pods

    kubectl scale deployment -n day-3 --replicas {quantidade-replicas} nginx-deployment #alterando quantidade replicas