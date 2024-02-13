# DevSecOps Pratices

## Préréquis

* Installer git, terraform, aws-cli
```console
delbechir@bngameni:~$ git --version
git version 2.43.0

delbechir@bngameni:~$ terraform --version
Terraform v1.6.5
on linux_amd64

delbechir@bngameni:~$ aws --version
aws-cli/2.14.5 Python/3.11.6 Linux/6.6.10-1-MANJARO exe/x86_64.manjaro.23 prompt/off
```
&nbsp;

* Disposer d'un compte aws fonctionnel(dans notre cas, les labs aws-learners sont largement suffisant) et le configurer via la cli aws-cli

```console
delbechir@bngameni:~$ aws configure --profile learner-lab
AWS Access Key ID [****************JFPV]: 
AWS Secret Access Key [****************tA5A]: 
Default region name [us-east-1]: 
Default output format [json]: 

delbechir@bngameni:~$ echo "aws_session_token=xxxxxxxxxxxxxxxxxxxxx" | tee -a ~/.aws/credentials
```

Ps: renseigner les valeurs aws key id, aws secret key, session token qui sont récuperables sur le lab.
&nbsp;




## Acces à la VM

1. Clone du repo forké
```console
delbechir@bngameni:~$ git clone git@github.com:bngameni/esgi-devsecops.git
```
&nbsp;



2. Se rendre dans le dossier terraform/aws à la racine du repo
```console
delbechir@bngameni:~$ cd esgi-devsecops/terraform/aws
```
&nbsp;


3. Initialiser terraform en télechargeant les données du provider
```console
delbechir@bngameni:~/esgi-devsecops/terraform/aws$ terraform init
```
&nbsp;

4. Executer terraform apply pour deployer l'infra
```console
delbechir@bngameni:~/esgi-devsecops/terraform/aws$ terraform apply -auto-approve
```
&nbsp;

5. Recuperer l'ip de la machine depuis le output terraform
```console
delbechir@bngameni:~/esgi-devsecops/terraform/aws$ terraform output -raw ec2instance_ip
3.X.X.X
```
&nbsp;

6. Se connecter en ssh au serveur en ayant au préalable telecharger la clé ssh du lab.
```console
delbechir@bngameni:~/esgi-devsecops/terraform/aws$ ssh -i labsuser.pem ubuntu@3.X.X.X
Welcome to Ubuntu 22.04.3 LTS (GNU/Linux 6.2.0-1012-aws x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Mon Feb 12 13:48:12 UTC 2024

  System load:  0.240234375        Processes:                129
  Usage of /:   14.6% of 28.89GB   Users logged in:          1
  Memory usage: 32%                IPv4 address for docker0: 172.17.0.1
  Swap usage:   0%                 IPv4 address for eth0:    10.0.1.61


Expanded Security Maintenance for Applications is not enabled.

109 updates can be applied immediately.
68 of these updates are standard security updates.
To see these additional updates run: apt list --upgradable

Enable ESM Apps to receive additional future security updates.
See https://ubuntu.com/esm or run: sudo pro status


Last login: Mon Feb 12 13:22:59 2024 from 94.228.190.38
ubuntu@ip-10-0-1-61:~$ 
```


Ps: 3.X.X.X represente l'ip publique de la machine et labuser.pem represente la clé ssh.
