# Day-9
Esta etapa o foco será o Ingress, neste caso os exemplos seram com o **Ingress Nginx Controller**.
No entando este Controller não terá (ou não tem mais) suporte, O subistituto para ele é o **Gateway API**.
Futuramente seram adicionados exemplos com o **Gateway API**.

Para melhor entendimento consultar a doc oficial do k8s:
- https://kubernetes.io/docs/concepts/services-networking/ingress/

O Gateway API:
- https://kubernetes.io/docs/concepts/services-networking/gateway/

Extra sobre Ingress Nginx Controller:
- https://kubernetes.io/blog/2025/11/11/ingress-nginx-retirement/
- https://github.com/kubernetes/ingress-nginx

## Ingress
Ingress faz o controle do acesso externo para os servicos dentro do cluster.

## Instalando o Ingress Nginx Controller (no kind)

	kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml

Para desinstalar basta usar o delete.


## Comandos

    kubectl apply -f .
    kubectl get ingress
    kubectl get pods
    kubectl get service
    kubectl describe ingress
    kubectl delete -f .

Configuracao do Kind (importante estar no diretorio com o arquivo de config):

    kind create cluster --config template.yaml --name nome
