
Projeto voltado para observabilidade.
===
Repo voltado para projetos de infra e observabilidade


# Inicio de tudo

### A ideia inicial do projeto era apenas ter nesse repo os arquivos .tf

`Aonde vai ser configurado VPC que contem uma subnet, security group, tabela de roteamento, Internet Gateway e uma EC2.`

### Ai assim que postei a ideia do projeto no linkedin eu pensei, porque não fazer algo a mais? E me surgiu a ideia de subir o zabbix usando o ansible.


# Parte 2

### A segunda parte desse projeto foi justamente, como vou subir o zabbix usando o ansible? Logo de incio fui pesquisar a documentação oficial do zabbix e vi que era algo simples de se fazer, basicamente foi um copia e cola dos comandos que são disponibilizados no site para modulos ansible, demorei umas 2hrs para fazer esse "copia e cola", por que fazia muito tempo que não usava o Ansible em si.

`Depois desse tempo fui refinar o código pensando oque falta? Ai me liguei que por padrão não vem configurado o mysql no servidor que estava subindo, a partir disso corri atrás de como vou instalar e condifgurar o mysql para o zabbix? depois de muitas horas de pesquisa e tentativas e erros eu consegui fazer uma role que instala e configura o mysql.`


# Como usar ?

## Entende-se que na sua maquina já tenha configurado o aws-cli e tenha uma chave ssh cadastrada na aws siga os passos abaixo:

`Entre no diretorio` [infratf](./infratf/)

### De inicio e preciso criar a nossa infra na aws, podemos utilizar o comando a seguir para subir a infra citada a cima:

```Bash
# Exemplo de comando para executar o .tf

$ terraform apply --auto-approve 

# Particulamente gosto de usar o argumento --auto-approve
```

`Após a criação da infra vai ser cuspido no terminal os IP's privados e publicos como no exemplo abaixo: `

```Bash

Apply complete! Resources: 8 added, 0 changed, 0 destroyed.

Outputs:

ip_privado = "10.0.1.143" # Apenas um exemplo!
ip_publico = "54.210.43.205" # Apenas um exemplo!


```
## Após isso copie o IP publico e cole no arquivo [hosts.yml](./infratf/Ansible/hosts.yml)

`Exemplo:`

```yml

[ec2-user]
54.210.43.205

```

## Com o arquivo [Hosts](./infratf/Ansible/hosts.yml) "configurado" está na hora de rodar o nosso [playbook.yml](./infratf/Ansible/playbook.yml).

`Exemplo:`

```bash

$  ansible-playbook playbook.yml -u ec2-user --private-key id_rsa.pem -i hosts.yml 

# Os argumentos utlizados acima são:
# -u de usuario que por padrão vamos usar o ec2-user
# --private-key a chave privada cadastrada na criação da ec2
# -i aponta o arquivo hosts.yml mostrando o ip dos servidores que vão ser afetados pelo playbook

```
## De final e jogar o ip publico da nossa maquina no navegador "54.210.43.205/zabbix" e fazer a configuração inicial do zabbix