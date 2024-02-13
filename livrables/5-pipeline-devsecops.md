# Pipeline DevSecOps

## Application

* Fork de l'application
    ![Fork de l'app Java](./images/5.1-fork.png)

&nbsp;
&nbsp;


## JDK & Maven

* Installation du plugin temurin
    ![Temurin Install](./images/5.2-temurin-install.png)
&nbsp;

* Configuration du jdk dans jenkins
    ![Config jdk](./images/5.3-config-jdk.png)
&nbsp;

* Configuration de maven dans jenkins
    ![Config maven](./images/5.4-config-maven.png)
&nbsp;

&nbsp;


## Pipeline
Documentation here: https://esgi.gitbook.io/devsecops/exercice/etape-1-maven

* Création d'un item dans jenknis
    ![Pipeline jenkins creation](./images/5.5-create-pipeline.png)
&nbsp;

* Configuration du pipeline
    ![Pipeline config](./images/5.6-set-pipeline-script.png)
&nbsp;


* Build du pipeline
    ![Pipeline build](./images/5.7-pipeline-logs-build.png)
&nbsp;


* Status du build
    ![Pipeline status](./images/5.8-pipeline-status.png)
&nbsp;

&nbsp;


## Analyse Sonarqube

Documentation here: https://esgi.gitbook.io/devsecops/exercice/etape-2-analyse-sonarqube

* Installation du plugin Sonarqube scanner
    ![Install plugin sonarqube scanner](./images/5.9-sonarqube-plugin-install.png)
&nbsp;


* Creation d'un token admin sur sonaqube
    ![Token creation](./images/5.10-sonarqube-create-token.png)
    ![Token created](./images/5.11-sonarqube-token-created.png)
&nbsp;


* Ajout du token dans jenkins comme credentials
    ![Credentials ajoutés](./images/5.13-credentials-added.png)
&nbsp;


* Configuration du serveur sonarqube dans jenkins
    ![Add install sonarqube](./images/5.14-config-sonarqube-jenkins.png)
    Ps: Utilisation du localhost car sonarqube et jenkins sont installés sur le même serveur
&nbsp;

* Configuration du plugin sonarqube scanner
    ![plugin sonar scanner](./images/5.15-config-sonnar-scanner.png)
&nbsp;


* Ajout d'une quality gate dans sonarqube
    ![Quality gate](./images/5.16-ajout-du-webhook.png)
&nbsp;


* Modification de la definition du pipeline
    ![Update du pipeline](./images/5.17-update-du-pipeline.png)
&nbsp;

* Logs du build après mise à jour
    ![Logs after update](./images/5.18-logs-after-update.png)    
&nbsp;


* Status du build
    ![Status after update](./images/5.19-status-after-update.png)
&nbsp;

* Resultats d'analyses sur sonarqube
    ![Resultas analyses sonarqube](./images/5.20-sonarqube-analysis.png)
&nbsp;

&nbsp;


## Dependances OWASP

* Installation du plugin dependency check
    ![Install plugin owasp dependency-check](./images/5.21-owasp-plugin-install.png)
&nbsp;

* Configuration du plugin
    ![Configure plugin owasp dependency-check](./images/5.22-owasp-dp-check-config.png)
&nbsp;

* Ajout du stage de check de dependance dans la pipeline
    ![Add Stage Check Dependency](./images/5.23-add-dp-check-stage.png)
&nbsp;


* Status de la pipeline
    ![Status de la pipeline](./images/5.24-dp-check-status-pipeline.png)
&nbsp;

* Show du dependency check dans jenkins
    ![Dependency check](./images/5.25-dp-check-render.png)
&nbsp;

&nbsp;


## Deploiement Docker

* Installation des plugins dockers
    ![Install docker plugins](./images/5.26-docker-plugins.png)
&nbsp;

* Configuration des plugins
    ![Config plugins](./images/5.27-config-docker-plugins.png)
&nbsp;

* Set docker hub credentials
    ![Config credentials](./images/5.28-set-docker-creds.png)
&nbsp;

* Add build, scan with trivy and deploy stage
    ![Add stages](./images/5.29-add-stages.png)
&nbsp;


* Resultat du pipeline
    ![Pipeline Status](./images/5.30-build-terminated.png)
&nbsp;


* Image dans le docker hub
    ![Pipeline Status](./images/5.31-image-in-docker-hub.png)
&nbsp;


* Access to application
    ![Access application](./images/5.32-access-to-app.png)
&nbsp;

&nbsp;


Note: Les IPS ont changé après restart du lab

## Deploiement sur Kubernetes
Docs: https://esgi.gitbook.io/devsecops/exercice/etape-5-deploiement-kubernetes

* Installation des plugins
    ![Plugins Kubernetes](./images/5.33-kube-plugins.png)
&nbsp;

* Ajout des credentials
    ![Creds kube](./images/5.34-kube-creds.png)
&nbsp;

* Install de kubectl
    ```console
    ubuntu@ip-10-0-1-61:~$ sudo apt update
    sudo apt install curl
    curl -LO https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl
    sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
    kubectl version --client

    Hit:1 http://us-east-1.ec2.archive.ubuntu.com/ubuntu jammy InRelease
    Get:2 http://us-east-1.ec2.archive.ubuntu.com/ubuntu jammy-updates InRelease [119 kB]                                                         
    Hit:3 http://us-east-1.ec2.archive.ubuntu.com/ubuntu jammy-backports InRelease                                                                
    Hit:4 https://aquasecurity.github.io/trivy-repo/deb jammy InRelease                                                                                          
    Ign:5 https://pkg.jenkins.io/debian-stable binary/ InRelease                                                                                                 
    Hit:6 https://pkg.jenkins.io/debian-stable binary/ Release                                           
    Get:7 http://security.ubuntu.com/ubuntu jammy-security InRelease [110 kB]
    Get:8 http://us-east-1.ec2.archive.ubuntu.com/ubuntu jammy-updates/main amd64 Packages [1371 kB]
    Get:9 http://us-east-1.ec2.archive.ubuntu.com/ubuntu jammy-updates/universe amd64 Packages [1046 kB]
    Get:11 http://security.ubuntu.com/ubuntu jammy-security/main amd64 Packages [1154 kB]    
    Get:12 http://security.ubuntu.com/ubuntu jammy-security/main Translation-en [212 kB]
    Get:13 http://security.ubuntu.com/ubuntu jammy-security/restricted amd64 Packages [1392 kB]
    Get:14 http://security.ubuntu.com/ubuntu jammy-security/restricted Translation-en [229 kB]
    Fetched 5633 kB in 2s (3346 kB/s)                                  
    Reading package lists... Done
    Building dependency tree... Done
    Reading state information... Done
    50 packages can be upgraded. Run 'apt list --upgradable' to see them.
    W: Target Packages (main/binary-amd64/Packages) is configured multiple times in /etc/apt/sources.list.d/trivy.list:1 and /etc/apt/sources.list.d/trivy.list:2
    W: Target Packages (main/binary-all/Packages) is configured multiple times in /etc/apt/sources.list.d/trivy.list:1 and /etc/apt/sources.list.d/trivy.list:2
    W: Target Translations (main/i18n/Translation-en) is configured multiple times in /etc/apt/sources.list.d/trivy.list:1 and /etc/apt/sources.list.d/trivy.list:2
    W: Target CNF (main/cnf/Commands-amd64) is configured multiple times in /etc/apt/sources.list.d/trivy.list:1 and /etc/apt/sources.list.d/trivy.list:2
    W: Target CNF (main/cnf/Commands-all) is configured multiple times in /etc/apt/sources.list.d/trivy.list:1 and /etc/apt/sources.list.d/trivy.list:2
    W: Target Packages (main/binary-amd64/Packages) is configured multiple times in /etc/apt/sources.list.d/trivy.list:1 and /etc/apt/sources.list.d/trivy.list:2
    W: Target Packages (main/binary-all/Packages) is configured multiple times in /etc/apt/sources.list.d/trivy.list:1 and /etc/apt/sources.list.d/trivy.list:2
    W: Target Translations (main/i18n/Translation-en) is configured multiple times in /etc/apt/sources.list.d/trivy.list:1 and /etc/apt/sources.list.d/trivy.list:2
    W: Target CNF (main/cnf/Commands-amd64) is configured multiple times in /etc/apt/sources.list.d/trivy.list:1 and /etc/apt/sources.list.d/trivy.list:2
    W: Target CNF (main/cnf/Commands-all) is configured multiple times in /etc/apt/sources.list.d/trivy.list:1 and /etc/apt/sources.list.d/trivy.list:2
    Reading package lists... Done
    Building dependency tree... Done
    Reading state information... Done
    curl is already the newest version (7.81.0-1ubuntu1.15).
    0 upgraded, 0 newly installed, 0 to remove and 50 not upgraded.
    % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                    Dload  Upload   Total   Spent    Left  Speed
    100   138  100   138    0     0   1396      0 --:--:-- --:--:-- --:--:--  1408
    100 47.4M  100 47.4M    0     0  86.5M      0 --:--:-- --:--:-- --:--:--  134M
    Client Version: v1.29.1
    Kustomize Version: v5.0.4-0.20230601165947-6ce0bf390ce3
    ```

* Modification de l'image dans le manifest kube
    ![Deployment update kube](./images/5.35-update-image-k8s.png)
&nbsp;

* Mise à jour du pipeline en rajoutant le stage de deploiement sur k8s
    ![Stage k8s](./images/5.36-add-k8s-stage.png)
&nbsp;


* Status du build
    Ps: Pipeline precedent supprimé par erreur, nouveau pipeline crée pour la cause
    ![Status build](./images/5.37-k8s-build-status.png)
    

* Log de deploiement
    ![Logs deploy](./images/5.38-k8s-logs-deploy.png)