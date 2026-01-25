# Day-8
Nesta etapa do treinamento o estudo será direcionado sobre Secrets e ConfigMap que 
responsáveis por armazenar as informações sensíveis da sua aplicação

Para melhor entendimento consultar a doc oficial do k8s:
- https://kubernetes.io/docs/concepts/configuration/secret/
- https://kubernetes.io/docs/concepts/configuration/configmap/

## Secrets
O objeto **Secret** é utilizado no Kubernetes para armazenar **informações sensíveis** em pequenos volumes de dados, como **senhas, chaves, tokens e credenciais**.

Por padrão, os dados armazenados em um Secret são codificados em **Base64**, o que **não representa criptografia** e, portanto, **não garante segurança criptográfica**. Essa codificação serve apenas para transporte e armazenamento padronizado dos dados.

Os Secrets são armazenados no **etcd**, o banco de dados chave-valor do cluster Kubernetes, e podem ser consumidos por **Pods**, seja como **variáveis de ambiente** ou **arquivos montados em volumes**, além de também poderem ser utilizados pelo próprio sistema.

## ConfigMap
O objeto **ConfigMap** é utilizado para armazenar **dados de configuração não sensíveis**, como **variáveis de ambiente**, **parâmetros de aplicação** e **arquivos de configuração**.

Os ConfigMaps permitem **desacoplar a configuração da aplicação da imagem do container**, tornando possível utilizar a **mesma imagem** em diferentes ambientes (desenvolvimento, homologação e produção) apenas alterando os valores de configuração.

Essa abordagem melhora a **manutenibilidade**, **portabilidade** e **flexibilidade** das aplicações executadas no Kubernetes.

## Importante

Neste arquivo, **por motivos didáticos**, os arquivos do certificado TLS estão armazenados junto ao restante do conteúdo do projeto.  
Em ambientes reais, essa prática **não é recomendada**, pois certificados e chaves privadas são **informações sensíveis** e exigem cuidados adicionais de segurança.

### Boas práticas para armazenamento de certificados TLS
Em cenários de produção, o mais comum é:

- **Não versionar certificados e chaves privadas** em repositórios Git
- Armazenar certificados e chaves em **Secrets do Kubernetes**
- Utilizar **serviços de gerenciamento de segredos**, como:
  - AWS Secrets Manager
  - Azure Key Vault
  - Google Secret Manager
  - HashiCorp Vault

### Estrutura comum em projetos
Quando necessário manter arquivos localmente (apenas para testes ou automação), costuma-se utilizar diretórios dedicados


## Comandos

    echo -n "string-em-base64" | base64 -d 
    # Decodifica uma string que está em Base64 (converte para o texto original)
    
    echo -n "string" | base64 
    # Codifica uma string em Base64
    
    kubectl get secrets
    # Lista todos os Secrets existentes no namespace atual do Kubernetes
    
    kubectl describe secrets secret-01
    # Mostra detalhes completos do Secret chamado "secret-01" (metadados, tipo, eventos etc.)
    
    kubectl apply -f secret-01.yaml
    # Cria ou atualiza um Secret a partir do arquivo YAML informado
    
    kubectl create secret generic secret-cli --from-literal=username=dXNlcg== --from-literal=password=c2VuaGE=
    # Cria um Secret genérico via CLI, definindo valores diretamente (já codificados em Base64)
    
    kubectl create secret tls secret-web-tls --cert=certificado.crt --key=chave-privada.key
    # Cria um Secret TLS contendo certificado e chave privada para HTTPS
    
    kubectl create configmap nginx-conf --from-file nginx/nginx.conf
    # Cria um ConfigMap a partir de um arquivo de configuração do Nginx
    
    kubectl create configmap nginx-conf --from-file nginx/nginx.conf --dry-run=client -o yaml > configmap-web-tls.yaml
    # Gera o YAML do ConfigMap sem criar no cluster e salva em um arquivo
    
    kubectl exec nome-pod -- env 
    # Executa o comando "env" dentro do Pod para listar variáveis de ambiente
    
    kubectl port-forward pod-configmap-secret 4443:443 
    # Encaminha a porta 4443 da máquina local para a porta 443 do Pod (acesso local ao serviço HTTPS)


Vericando credencias do docker:

    docker login
    # Autentica no Docker Registry (ex: Docker Hub) e, se já estiver logado, valida a sessão e mostra o usuário autenticado

Exemplo de comando para certificado TLS:

    openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout chave-privada.key -out certificado.crt
    # Gera um certificado SSL autoassinado (X.509) válido por 365 dias com chave RSA de 2048 bits,
    # criando a chave privada (chave-privada.key) e o certificado público (certificado.crt)





