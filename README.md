# Mini Testbeds 🚀

Projeto de demonstração que simula em escala reduzida o funcionamento do **Serviço de Testbeds da RNP** — plataforma dedicada à experimentação e inovação em tecnologias de rede e computação.

## Sobre o projeto

O Mini Testbeds provisiona automaticamente um cluster Kubernetes local com aplicação deployada, simulando o fluxo real de disponibilização de ambientes de experimentação para pesquisadores.

## Tecnologias utilizadas

| Tecnologia | Função |
|---|---|
| Ansible | Validação e configuração do ambiente |
| kind | Cluster Kubernetes local via Docker |
| Kubernetes | Orquestração de containers |
| Helm | Gerenciamento e deploy da aplicação |
| GitHub Actions | Pipeline CI/CD automatizada |
| nginx | Aplicação de demonstração |

## Pipeline CI/CD

A pipeline é composta por 3 jobs executados em sequência:

- **Validar** — Ansible verifica o ambiente + Helm lint valida o chart
- **Provisionar** — kind cria o cluster + Helm instala a aplicação
- **Testar** — curl valida que a aplicação está respondendo

## Como executar localmente

### Pré-requisitos
- Docker
- kubectl
- kind
- Helm
- Ansible

### Passo a passo

1. Clonar o repositório
   git clone https://github.com/Pedro1p0/mini-testbeds.git
   cd mini-testbeds

2. Validar o ambiente com Ansible
   ansible-playbook -i ansible/inventory.ini ansible/playbook.yml

3. Criar o cluster Kubernetes
   kind create cluster --config kind/cluster-config.yaml

4. Verificar os nós
   kubectl get nodes

5. Instalar a aplicação via Helm
   helm install meu-testbed helm/app

6. Aguardar os pods ficarem prontos
   kubectl get pods -w

7. Testar a aplicação
   kubectl run curl-test --image=curlimages/curl --rm -it --restart=Never -- curl -s http://meu-testbed-service:80

8. Limpar o ambiente
   kind delete cluster --name mini-testbed

## Conexão com o projeto Testbeds RNP

Este projeto foi desenvolvido como demonstração prática dos conceitos aplicados no projeto **Manutenção Evolutiva Testbeds — Clusters Kubernetes On Demand** da RNP, que visa provisionar clusters Kubernetes virtualizados e geograficamente distribuídos para experimentação científica.

| Mini Testbeds (este projeto) | Testbeds RNP |
|---|---|
| kind cria nós via Docker | KVM cria VMs em servidores físicos |
| 1 máquina local | Servidores distribuídos pelo Brasil |
| Cluster único | Clusters sob demanda por pesquisador |
| nginx como app demo | Ambientes de experimentação reais |

## Autor

Pedro Vitor Clemente
Engenheiro de Computação — UFRN
GitHub: https://github.com/Pedro1p0
Email: pedro1p0@outlook.com