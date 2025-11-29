# Desafio

O desafio consiste em executar o pod-faminto em um namespace específico para entender melhor o seu funcionamento.

## Proposições

1 - Quando o Pod foi morto, qual mecanismo do Linux foi acionado pelo Kubernetes/Container Runtime para parar o processo?
2 - Qual a diferença prática entre definir um request de 100Mi e um limit de 200Mi? O que o Scheduler usa para decidir onde colocar o Pod?

## Resposta

1 - O processo Linux é parado pelo Out of Memory Killer (OOMKiller). O Kubernetes não mata nenhum processo diretamente; ele delega essa função ao sistema operacional. Quando o container ultrapassa o limite de memória configurado, o cgroup associado ao container detecta o excesso e aciona o OOMKiller, que finaliza o processo.
Como o Pod está configurado com "restartPolicy: Always", o kubelet detecta que o container morreu e reinicia o container automaticamente.

Isso pode ser lido e entendido melhor na documentação do Kubernetes: https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle

2 - A diferença prática é que o request é o valor mínimo de recursos que o container precisa para ser criado e agendado. Ou seja, o Scheduler só coloca o Pod em um Node que tenha pelo menos esse valor disponível. Já o limit define o valor máximo de consumo permitido para o container; se ele exceder esse limite, poderá ser finalizado pelo OOMKiller.
O Scheduler usa dois passos para decidir onde colocar o Pod: filtering e scoring.
No filtering (filtragem), ele identifica os Nodes que possuem recursos suficientes e atendem aos requisitos do Pod.
No scoring, ele atribui pontuações aos Nodes viáveis para escolher o mais adequado.
Após esse processo, o kube-scheduler atribui o Pod ao Node mais apropriado. Caso haja pontuações iguais, o kube-scheduler pode escolher aleatoriamente entre os Nodes empatados.

Isso pode ser lido e entendido melhor na documentação do Kubernetes: https://kubernetes.io/docs/concepts/scheduling-eviction/kube-scheduler/

## Comandos
Comando para criar o namespace:

    kubectl create namespace treinamento-ch2 --dry-run=client -o yaml > namesapce.yaml

Para cirar o namespace poderia ter usado o comando abaixo, mas para manter tudo no mesmo padrão optei por usar o manifesto namesapce.yaml

    kubectl create namespace treinamento-ch2

Comando para aplicar o namespace:

    kubectl apply -f namesapce.yaml

Comando para criar o Pod:

    kubectl run pod-faminto --image polinux/stress --dry-run=client -o yaml -n treinamento-ch2 > pod-faminto.yaml
    kubectl run pod-faminto-resolvido --image polinux/stress --dry-run=client -o yaml -n treinamento-ch2 > pod-faminto-resolvido.yaml

Comando para aplicar os pods:

    kubectl apply -f pod-faminto-resolvido.yaml
    kubectl apply -f pod-faminto.yaml

Comando para ver a criação do Pod:

    kubectl get pods -n treinamento-ch2 -w