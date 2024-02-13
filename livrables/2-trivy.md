# Trivy Practices


## Installation de Trivy

* Installation des dépendances
```console
delbechir@bngameni:~$ sudo apt-get install wget apt-transport-https gnupg lsb-release -y

Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
lsb-release is already the newest version (11.1.0ubuntu4).
wget is already the newest version (1.21.2-2ubuntu1).
gnupg is already the newest version (2.2.27-3ubuntu2.1).
The following NEW packages will be installed:
  apt-transport-https
0 upgraded, 1 newly installed, 0 to remove and 104 not upgraded.
Need to get 1510 B of archives.
After this operation, 170 kB of additional disk space will be used.
Get:1 http://us-east-1.ec2.archive.ubuntu.com/ubuntu jammy-updates/universe amd64 apt-transport-https all 2.4.11 [1510 B]
Fetched 1510 B in 0s (88.6 kB/s)              
Selecting previously unselected package apt-transport-https.
(Reading database ... 81508 files and directories currently installed.)
Preparing to unpack .../apt-transport-https_2.4.11_all.deb ...
Unpacking apt-transport-https (2.4.11) ...
Setting up apt-transport-https (2.4.11) ...
Scanning processes...                                                                                                                                          
Scanning linux images...                                                                                                                                       

Running kernel seems to be up-to-date.

No services need to be restarted.

No containers need to be restarted.

No user sessions are running outdated binaries.

No VM guests are running outdated hypervisor (qemu) binaries on this host.

```
&nbsp;

* Configuration du repo

```console
delbechir@bngameni:~$ wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | gpg --dearmor | sudo tee /usr/share/keyrings/trivy.gpg > /dev/null

delbechir@bngameni:~$ echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main" | sudo tee -a /etc/apt/sources.list.d/trivy.list

deb [signed-by=/usr/share/keyrings/trivy.gpg] https://aquasecurity.github.io/trivy-repo/deb jammy main
```
&nbsp;


* Mise à du jour du repositoire des paquets et installation de trivy
```console
delbechir@bngameni:~$ sudo apt-get update && sudo apt-get install trivy -y
Hit:1 http://us-east-1.ec2.archive.ubuntu.com/ubuntu jammy InRelease
Get:2 http://us-east-1.ec2.archive.ubuntu.com/ubuntu jammy-updates InRelease [119 kB]                                                       
Hit:3 http://us-east-1.ec2.archive.ubuntu.com/ubuntu jammy-backports InRelease                                                              
Get:4 http://security.ubuntu.com/ubuntu jammy-security InRelease [110 kB]                                                                   
Hit:5 https://aquasecurity.github.io/trivy-repo/deb jammy InRelease                                                                         
Ign:6 https://pkg.jenkins.io/debian-stable binary/ InRelease                                                                             
Hit:7 https://pkg.jenkins.io/debian-stable binary/ Release                       
Get:8 http://us-east-1.ec2.archive.ubuntu.com/ubuntu jammy-updates/main amd64 Packages [1371 kB]
Get:9 http://us-east-1.ec2.archive.ubuntu.com/ubuntu jammy-updates/main Translation-en [273 kB]
Get:10 http://us-east-1.ec2.archive.ubuntu.com/ubuntu jammy-updates/universe amd64 Packages [1046 kB]
Get:11 http://us-east-1.ec2.archive.ubuntu.com/ubuntu jammy-updates/universe Translation-en [236 kB]
Get:12 http://security.ubuntu.com/ubuntu jammy-security/main amd64 Packages [1152 kB]
Get:14 http://security.ubuntu.com/ubuntu jammy-security/main Translation-en [212 kB]
Get:15 http://security.ubuntu.com/ubuntu jammy-security/restricted amd64 Packages [1387 kB]
Get:16 http://security.ubuntu.com/ubuntu jammy-security/restricted Translation-en [228 kB]
Get:17 http://security.ubuntu.com/ubuntu jammy-security/universe amd64 Packages [839 kB]
Fetched 6973 kB in 1s (4702 kB/s)                          
Reading package lists... Done
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
trivy is already the newest version (0.49.1).
0 upgraded, 0 newly installed, 0 to remove and 110 not upgraded.
```
&nbsp;

* Verifier que trivy est bien installé
```console
delbechir@bngameni:~$ trivy --version
Version: 0.49.1
```



## Scan de code avec trivy

* Clone des repos après avoir forké.

```console
delbechir@bngameni:~$ git clone https://github.com/bngameni/terragoat.git
Cloning into 'terragoat'...
remote: Enumerating objects: 1087, done.
remote: Total 1087 (delta 0), reused 0 (delta 0), pack-reused 1087
Receiving objects: 100% (1087/1087), 706.58 KiB | 18.59 MiB/s, done.
Resolving deltas: 100% (522/522), done.

delbechir@bngameni:~$ git clone https://github.com/bngameni/esgi-devsecops.git
Cloning into 'esgi-devsecops'...
remote: Enumerating objects: 411, done.
remote: Counting objects: 100% (169/169), done.
remote: Compressing objects: 100% (116/116), done.
remote: Total 411 (delta 71), reused 134 (delta 42), pack-reused 242
Receiving objects: 100% (411/411), 7.61 MiB | 41.69 MiB/s, done.
Resolving deltas: 100% (178/178), done.
```

* Scan des fichiers
    ```console
    delbechir@bngameni:~$ cd esgi-devsecops/terraform
    ```
    &nbsp;

    * aws/ec2.tf
        ```console
        delbechir@bngameni:~/esgi-devsecops/terraform$ trivy config aws/ec2.tf 

        2024-02-12T16:47:25.475Z        INFO    Misconfiguration scanning is enabled
        2024-02-12T16:47:25.476Z        INFO    Need to update the built-in policies
        2024-02-12T16:47:25.476Z        INFO    Downloading the built-in policies...
        45.79 KiB / 45.79 KiB [------------------------------------------------------------------------------------------------------------------------------------------------------------] 100.00% ? p/s 0s
        2024-02-12T16:47:26.756Z        INFO    Detected config files: 2

        ec2.tf (terraform)

        Tests: 3 (SUCCESSES: 2, FAILURES: 1, EXCEPTIONS: 0)
        Failures: 1 (UNKNOWN: 0, LOW: 0, MEDIUM: 0, HIGH: 1, CRITICAL: 0)

        HIGH: Instance does not require IMDS access to require a token
        ═════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════

        IMDS v2 (Instance Metadata Service) introduced session authentication tokens which improve security when talking to IMDS.
        By default <code>aws_instance</code> resource sets IMDS session auth tokens to be optional. 
        To fully protect IMDS you need to enable session tokens by using <code>metadata_options</code> block and its <code>http_tokens</code> variable set to <code>required</code>.


        See https://avd.aquasec.com/misconfig/avd-aws-0028
        ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
        ec2.tf:1-20
        ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
        1 ┌ resource "aws_instance" "ci-cd_instance" {
        2 │   ami           = "ami-0fc5d935ebf8bc3bc"
        3 │   instance_type = "t2.large"
        4 │   key_name      = "vockey"
        5 │ 
        6 │   subnet_id                   = aws_subnet.ci-cd_subnet.id
        7 │   vpc_security_group_ids      = [aws_security_group.ci-cd_sg.id]
        8 │   associate_public_ip_address = true
        9 └ 
        ..   
        ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
        ```
        &nbsp;

      * aws/sg.tf
        ```console
        delbechir@bngameni:~/esgi-devsecops/terraform$ trivy config aws/sg.tf
        2024-02-12T16:50:19.159Z        INFO    Misconfiguration scanning is enabled
        2024-02-12T16:50:20.169Z        INFO    Detected config files: 2

        sg.tf (terraform)

        Tests: 11 (SUCCESSES: 6, FAILURES: 5, EXCEPTIONS: 0)
        Failures: 5 (UNKNOWN: 0, LOW: 0, MEDIUM: 0, HIGH: 0, CRITICAL: 5)

        CRITICAL: Security group rule allows egress to multiple public internet addresses.
        ═════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════
        Opening up ports to connect out to the public internet is generally to be avoided. You should restrict access to IP addresses or ranges that are explicitly required where possible.

        See https://avd.aquasec.com/misconfig/avd-aws-0104
        ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
        sg.tf:46
        via sg.tf:41-47 (egress)
            via sg.tf:4-48 (aws_security_group.ci-cd_sg)
        ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
        4   resource "aws_security_group" "ci-cd_sg" {
        .   
        46 [     cidr_blocks = ["0.0.0.0/0"]
        ..   
        48   }
        ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────


        CRITICAL: Security group rule allows ingress from public internet.
        ═════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════
        Opening up ports to the public internet is generally to be avoided. You should restrict access to IP addresses or ranges that explicitly require it where possible.

        See https://avd.aquasec.com/misconfig/avd-aws-0107
        ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
        sg.tf:38
        via sg.tf:33-39 (ingress)
            via sg.tf:4-48 (aws_security_group.ci-cd_sg)
        ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
        4   resource "aws_security_group" "ci-cd_sg" {
        .   
        38 [     cidr_blocks = ["0.0.0.0/0"]
        ..   
        48   }
        ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────


        CRITICAL: Security group rule allows ingress from public internet.
        ═════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════
        Opening up ports to the public internet is generally to be avoided. You should restrict access to IP addresses or ranges that explicitly require it where possible.

        See https://avd.aquasec.com/misconfig/avd-aws-0107
        ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
        sg.tf:30
        via sg.tf:25-31 (ingress)
            via sg.tf:4-48 (aws_security_group.ci-cd_sg)
        ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
        4   resource "aws_security_group" "ci-cd_sg" {
        .   
        30 [     cidr_blocks = ["0.0.0.0/0"]
        ..   
        48   }
        ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────


        CRITICAL: Security group rule allows ingress from public internet.
        ═════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════
        Opening up ports to the public internet is generally to be avoided. You should restrict access to IP addresses or ranges that explicitly require it where possible.

        See https://avd.aquasec.com/misconfig/avd-aws-0107
        ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
        sg.tf:22
        via sg.tf:17-23 (ingress)
            via sg.tf:4-48 (aws_security_group.ci-cd_sg)
        ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
        4   resource "aws_security_group" "ci-cd_sg" {
        .   
        22 [     cidr_blocks = ["0.0.0.0/0"]
        ..   
        48   }
        ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────


        CRITICAL: Security group rule allows ingress from public internet.
        ═════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════
        Opening up ports to the public internet is generally to be avoided. You should restrict access to IP addresses or ranges that explicitly require it where possible.

        See https://avd.aquasec.com/misconfig/avd-aws-0107
        ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
        sg.tf:14
        via sg.tf:9-15 (ingress)
            via sg.tf:4-48 (aws_security_group.ci-cd_sg)
        ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
        4   resource "aws_security_group" "ci-cd_sg" {
        .   
        14 [     cidr_blocks = ["0.0.0.0/0"]
        ..   
        48   }
        ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
        ```
        &nbsp;

        * azure/nsg.tf

        ```console
        delbechir@bngameni:~/esgi-devsecops/terraform$ trivy config azure/nsg.tf
        2024-02-12T16:50:19.159Z        INFO    Misconfiguration scanning is enabled
        2024-02-12T16:50:20.169Z        INFO    Detected config files: 2

        sg.tf (terraform)

        Tests: 11 (SUCCESSES: 6, FAILURES: 5, EXCEPTIONS: 0)
        Failures: 5 (UNKNOWN: 0, LOW: 0, MEDIUM: 0, HIGH: 0, CRITICAL: 5)

        CRITICAL: Security group rule allows egress to multiple public internet addresses.
        ═════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════
        Opening up ports to connect out to the public internet is generally to be avoided. You should restrict access to IP addresses or ranges that are explicitly required where possible.

        See https://avd.aquasec.com/misconfig/avd-aws-0104
        ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
        sg.tf:46
        via sg.tf:41-47 (egress)
            via sg.tf:4-48 (aws_security_group.ci-cd_sg)
        ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
        4   resource "aws_security_group" "ci-cd_sg" {
        .   
        46 [     cidr_blocks = ["0.0.0.0/0"]
        ..   
        48   }
        ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────


        CRITICAL: Security group rule allows ingress from public internet.
        ═════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════
        Opening up ports to the public internet is generally to be avoided. You should restrict access to IP addresses or ranges that explicitly require it where possible.

        See https://avd.aquasec.com/misconfig/avd-aws-0107
        ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
        sg.tf:38
        via sg.tf:33-39 (ingress)
            via sg.tf:4-48 (aws_security_group.ci-cd_sg)
        ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
        4   resource "aws_security_group" "ci-cd_sg" {
        .   
        38 [     cidr_blocks = ["0.0.0.0/0"]
        ..   
        48   }
        ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────


        CRITICAL: Security group rule allows ingress from public internet.
        ═════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════
        Opening up ports to the public internet is generally to be avoided. You should restrict access to IP addresses or ranges that explicitly require it where possible.

        See https://avd.aquasec.com/misconfig/avd-aws-0107
        ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
        sg.tf:30
        via sg.tf:25-31 (ingress)
            via sg.tf:4-48 (aws_security_group.ci-cd_sg)
        ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
        4   resource "aws_security_group" "ci-cd_sg" {
        .   
        30 [     cidr_blocks = ["0.0.0.0/0"]
        ..   
        48   }
        ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────


        CRITICAL: Security group rule allows ingress from public internet.
        ═════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════
        Opening up ports to the public internet is generally to be avoided. You should restrict access to IP addresses or ranges that explicitly require it where possible.

        See https://avd.aquasec.com/misconfig/avd-aws-0107
        ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
        sg.tf:22
        via sg.tf:17-23 (ingress)
            via sg.tf:4-48 (aws_security_group.ci-cd_sg)
        ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
        4   resource "aws_security_group" "ci-cd_sg" {
        ubuntu@ip-10-0-1-61:~/esgi-devsecops/terraform$ trivy config azure/nsg.tf
        2024-02-12T16:52:59.534Z        INFO    Misconfiguration scanning is enabled
        2024-02-12T16:53:00.574Z        INFO    Detected config files: 2

        nsg.tf (terraform)

        Tests: 4 (SUCCESSES: 2, FAILURES: 2, EXCEPTIONS: 0)
        Failures: 2 (UNKNOWN: 0, LOW: 0, MEDIUM: 0, HIGH: 0, CRITICAL: 2)

        CRITICAL: Security group rule allows ingress from public internet.
        ═════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════
        Network security rules should not use very broad subnets.

        Where possible, segments should be broken into smaller subnets.

        See https://avd.aquasec.com/misconfig/avd-azu-0047
        ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
        nsg.tf:16
        via nsg.tf:8-18 (security_rule)
            via nsg.tf:2-25 (azurerm_network_security_group.ci-cd-nsg)
        ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
        2   resource "azurerm_network_security_group" "ci-cd-nsg" {
        .   
        16 [     source_address_prefix      = "*"
        ..   
        25   }
        ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────


        CRITICAL: Security group rule allows ingress to SSH port from multiple public internet addresses.
        ═════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════
        SSH access can be configured on either the network security group or in the network security group rule. 

        SSH access should not be permitted from the internet (*, 0.0.0.0, /0, internet, any)

        See https://avd.aquasec.com/misconfig/avd-azu-0050
        ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
        nsg.tf:16
        via nsg.tf:8-18 (security_rule)
            via nsg.tf:2-25 (azurerm_network_security_group.ci-cd-nsg)
        ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
        2   resource "azurerm_network_security_group" "ci-cd-nsg" {
        .   
        16 [     source_address_prefix      = "*"
        ..   
        25   }
        ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
        ```
    &nbsp;


* Scan liés au repo terragoat
    ```console
    ubuntu@ip-10-0-1-61:~$ cd ~/terragoat
    ```

    * Scan du filesystem du dossier packages en recherchant uniquement celles qui sont critiques
    ```console
    ubuntu@ip-10-0-1-61:~~/terragoat$ trivy fs --severity "CRITICAL" ./packages/
    2024-02-12T17:02:46.481Z        INFO    Vulnerability scanning is enabled
    2024-02-12T17:02:46.482Z        INFO    Secret scanning is enabled
    2024-02-12T17:02:46.482Z        INFO    If your scanning is slow, please try '--scanners vuln' to disable secret scanning
    2024-02-12T17:02:46.482Z        INFO    Please see also https://aquasecurity.github.io/trivy/v0.49/docs/scanner/secret/#recommendation for faster secret detection
    2024-02-12T17:02:46.766Z        INFO    To collect the license information of packages in "node/base/package-lock.json", "npm install" needs to be performed beforehand
    2024-02-12T17:02:46.986Z        INFO    To collect the license information of packages in "node/twistcli_test/package-lock.json", "npm install" needs to be performed beforehand
    2024-02-12T17:02:47.070Z        INFO    Number of language-specific files: 6
    2024-02-12T17:02:47.070Z        INFO    Detecting npm vulnerabilities...
    2024-02-12T17:02:47.094Z        INFO    Detecting pom vulnerabilities...
    2024-02-12T17:02:47.094Z        INFO    Detecting pip vulnerabilities...

    node/base/package-lock.json (npm)

    Total: 8 (CRITICAL: 8)

    ┌──────────────────┬────────────────┬──────────┬──────────┬───────────────────┬────────────────────────────┬─────────────────────────────────────────────────────────────┐
    │     Library      │ Vulnerability  │ Severity │  Status  │ Installed Version │       Fixed Version        │                            Title                            │
    ├──────────────────┼────────────────┼──────────┼──────────┼───────────────────┼────────────────────────────┼─────────────────────────────────────────────────────────────┤
    │ @babel/traverse  │ CVE-2023-45133 │ CRITICAL │ fixed    │ 7.16.5            │ 7.23.2, 8.0.0-alpha.4      │ babel: arbitrary code execution                             │
    │                  │                │          │          │                   │                            │ https://avd.aquasec.com/nvd/cve-2023-45133                  │
    │                  │                │          │          ├───────────────────┤                            │                                                             │
    │                  │                │          │          │ 7.8.6             │                            │                                                             │
    │                  │                │          │          │                   │                            │                                                             │
    ├──────────────────┼────────────────┤          │          ├───────────────────┼────────────────────────────┼─────────────────────────────────────────────────────────────┤
    │ loader-utils     │ CVE-2022-37601 │          │          │ 1.4.0             │ 2.0.3, 1.4.1               │ loader-utils: prototype pollution in function parseQuery in │
    │                  │                │          │          │                   │                            │ parseQuery.js                                               │
    │                  │                │          │          │                   │                            │ https://avd.aquasec.com/nvd/cve-2022-37601                  │
    │                  │                │          │          ├───────────────────┤                            │                                                             │
    │                  │                │          │          │ 2.0.2             │                            │                                                             │
    │                  │                │          │          │                   │                            │                                                             │
    │                  │                │          │          │                   │                            │                                                             │
    ├──────────────────┼────────────────┤          │          ├───────────────────┼────────────────────────────┼─────────────────────────────────────────────────────────────┤
    │ minimist         │ CVE-2021-44906 │          │          │ 1.2.5             │ 1.2.6, 0.2.4               │ minimist: prototype pollution                               │
    │                  │                │          │          │                   │                            │ https://avd.aquasec.com/nvd/cve-2021-44906                  │
    ├──────────────────┼────────────────┤          │          ├───────────────────┼────────────────────────────┼─────────────────────────────────────────────────────────────┤
    │ socket.io-parser │ CVE-2022-2421  │          │          │ 4.0.4             │ 4.0.5, 4.2.1, 3.3.3, 3.4.2 │ Insufficient validation when decoding a Socket.IO packet    │
    │                  │                │          │          │                   │                            │ https://avd.aquasec.com/nvd/cve-2022-2421                   │
    ├──────────────────┼────────────────┤          │          ├───────────────────┼────────────────────────────┼─────────────────────────────────────────────────────────────┤
    │ webpack          │ CVE-2023-28154 │          │          │ 5.64.1            │ 5.76.0                     │ webpack: avoid cross-realm objects                          │
    │                  │                │          │          │                   │                            │ https://avd.aquasec.com/nvd/cve-2023-28154                  │
    ├──────────────────┼────────────────┤          ├──────────┼───────────────────┼────────────────────────────┼─────────────────────────────────────────────────────────────┤
    │ xmldom           │ CVE-2022-39353 │          │ affected │ 0.5.0             │                            │ Allows multiple root elements in a DOM tree                 │
    │                  │                │          │          │                   │                            │ https://avd.aquasec.com/nvd/cve-2022-39353                  │
    └──────────────────┴────────────────┴──────────┴──────────┴───────────────────┴────────────────────────────┴─────────────────────────────────────────────────────────────┘

    node/twistcli_test/package-lock.json (npm)

    Total: 1 (CRITICAL: 1)

    ┌─────────┬────────────────┬──────────┬──────────┬───────────────────┬───────────────┬─────────────────────────────────────────────┐
    │ Library │ Vulnerability  │ Severity │  Status  │ Installed Version │ Fixed Version │                    Title                    │
    ├─────────┼────────────────┼──────────┼──────────┼───────────────────┼───────────────┼─────────────────────────────────────────────┤
    │ xmldom  │ CVE-2022-39353 │ CRITICAL │ affected │ 0.5.0             │               │ Allows multiple root elements in a DOM tree │
    │         │                │          │          │                   │               │ https://avd.aquasec.com/nvd/cve-2022-39353  │
    └─────────┴────────────────┴──────────┴──────────┴───────────────────┴───────────────┴─────────────────────────────────────────────┘

    requirements.txt (pip)

    Total: 2 (CRITICAL: 2)

    ┌─────────┬────────────────┬──────────┬────────┬───────────────────┬────────────────────────┬───────────────────────────────────────────────────────┐
    │ Library │ Vulnerability  │ Severity │ Status │ Installed Version │     Fixed Version      │                         Title                         │
    ├─────────┼────────────────┼──────────┼────────┼───────────────────┼────────────────────────┼───────────────────────────────────────────────────────┤
    │ django  │ CVE-2019-19844 │ CRITICAL │ fixed  │ 1.2               │ 1.11.27, 2.2.9, 3.0.1  │ Django: crafted email address allows account takeover │
    │         │                │          │        │                   │                        │ https://avd.aquasec.com/nvd/cve-2019-19844            │
    │         ├────────────────┤          │        │                   ├────────────────────────┼───────────────────────────────────────────────────────┤
    │         │ CVE-2020-7471  │          │        │                   │ 1.11.28, 2.2.10, 3.0.3 │ potential SQL injection via StringAgg(delimiter)      │
    │         │                │          │        │                   │                        │ https://avd.aquasec.com/nvd/cve-2020-7471             │
    └─────────┴────────────────┴──────────┴────────┴───────────────────┴────────────────────────┴───────────────────────────────────────────────────────┘

    sub/.hidden/requirements.txt (pip)

    Total: 2 (CRITICAL: 2)

    ┌─────────┬────────────────┬──────────┬────────┬───────────────────┬────────────────────────┬───────────────────────────────────────────────────────┐
    │ Library │ Vulnerability  │ Severity │ Status │ Installed Version │     Fixed Version      │                         Title                         │
    ├─────────┼────────────────┼──────────┼────────┼───────────────────┼────────────────────────┼───────────────────────────────────────────────────────┤
    │ django  │ CVE-2019-19844 │ CRITICAL │ fixed  │ 1.2               │ 1.11.27, 2.2.9, 3.0.1  │ Django: crafted email address allows account takeover │
    │         │                │          │        │                   │                        │ https://avd.aquasec.com/nvd/cve-2019-19844            │
    │         ├────────────────┤          │        │                   ├────────────────────────┼───────────────────────────────────────────────────────┤
    │         │ CVE-2020-7471  │          │        │                   │ 1.11.28, 2.2.10, 3.0.3 │ potential SQL injection via StringAgg(delimiter)      │
    │         │                │          │        │                   │                        │ https://avd.aquasec.com/nvd/cve-2020-7471             │
    └─────────┴────────────────┴──────────┴────────┴───────────────────┴────────────────────────┴───────────────────────────────────────────────────────┘

    sub/pom.xml (pom)

    Total: 2 (CRITICAL: 2)

    ┌─────────────────────────────────────┬────────────────┬──────────┬────────┬───────────────────┬───────────────────────┬──────────────────────────────────────────────────────────┐
    │               Library               │ Vulnerability  │ Severity │ Status │ Installed Version │     Fixed Version     │                          Title                           │
    ├─────────────────────────────────────┼────────────────┼──────────┼────────┼───────────────────┼───────────────────────┼──────────────────────────────────────────────────────────┤
    │ org.apache.logging.log4j:log4j-core │ CVE-2021-44228 │ CRITICAL │ fixed  │ 2.14.0            │ 2.15.0, 2.3.1, 2.12.2 │ Remote code execution in Log4j 2.x when logs contain an  │
    │                                     │                │          │        │                   │                       │ attacker-controlled string...                            │
    │                                     │                │          │        │                   │                       │ https://avd.aquasec.com/nvd/cve-2021-44228               │
    │                                     ├────────────────┤          │        │                   ├───────────────────────┼──────────────────────────────────────────────────────────┤
    │                                     │ CVE-2021-45046 │          │        │                   │ 2.16.0, 2.12.2        │ log4j-core: DoS in log4j 2.x with thread context message │
    │                                     │                │          │        │                   │                       │ pattern and context...                                   │
    │                                     │                │          │        │                   │                       │ https://avd.aquasec.com/nvd/cve-2021-45046               │
    └─────────────────────────────────────┴────────────────┴──────────┴────────┴───────────────────┴───────────────────────┴──────────────────────────────────────────────────────────┘
    ```

    * Afficher les vulnérabilités liées à django
    ```console
    ubuntu@ip-10-0-1-61:~/terragoat$ trivy fs ./packages/ | grep "django"
    2024-02-12T17:06:11.905Z        INFO    Vulnerability scanning is enabled
    2024-02-12T17:06:11.905Z        INFO    Secret scanning is enabled
    2024-02-12T17:06:11.905Z        INFO    If your scanning is slow, please try '--scanners vuln' to disable secret scanning
    2024-02-12T17:06:11.905Z        INFO    Please see also https://aquasecurity.github.io/trivy/v0.49/docs/scanner/secret/#recommendation for faster secret detection
    2024-02-12T17:06:12.181Z        INFO    To collect the license information of packages in "node/base/package-lock.json", "npm install" needs to be performed beforehand
    2024-02-12T17:06:12.399Z        INFO    To collect the license information of packages in "node/twistcli_test/package-lock.json", "npm install" needs to be performed beforehand
    2024-02-12T17:06:12.486Z        INFO    Number of language-specific files: 6
    2024-02-12T17:06:12.486Z        INFO    Detecting npm vulnerabilities...
    2024-02-12T17:06:12.507Z        INFO    Detecting pom vulnerabilities...
    2024-02-12T17:06:12.507Z        INFO    Detecting pip vulnerabilities...
    │ django  │ CVE-2019-19844 │ CRITICAL │ fixed  │ 1.2               │ 1.11.27, 2.2.9, 3.0.1  │ Django: crafted email address allows account takeover        │
    │         │ CVE-2011-0698  │ HIGH     │        │                   │ 1.1.4, 1.2.5           │ High severity vulnerability that affects django              │
    │         │ CVE-2016-2512  │          │        │                   │ 1.8.10, 1.9.3          │ python-django: Malicious redirect and possible XSS attack    │
    │         │ CVE-2016-7401  │          │        │                   │ 1.8.15, 1.9.10         │ python-django: CSRF protection bypass on a site with Google  │
    │         │ CVE-2019-6975  │          │        │                   │ 1.11.19, 2.0.11, 2.1.6 │ python-django: memory exhaustion in                          │
    │         │                │          │        │                   │                        │ django.utils.numberformat.format()                           │
    │         │ CVE-2020-9402  │          │        │                   │ 1.11.29, 2.2.11, 3.0.4 │ django: potential SQL injection via "tolerance" parameter in │
    │         │ CVE-2010-4534  │          │        │                   │ 1.1.3, 1.2.4           │ The administrative interface in django.contrib.admin in      │
    │         │ CVE-2010-4535  │          │        │                   │                        │ The password reset functionality in django.contrib.auth in   │
    │         │ CVE-2011-4136  │          │        │                   │ 1.3.1, 1.2.7           │ django.contrib.sessions in Django before 1.2.7 and 1.3.x     │
    │         │ CVE-2012-3442  │          │        │                   │ 1.3.2, 1.4.1           │ The (1) django.http.HttpResponseRedirect and (2)             │
    │         │                │          │        │                   │                        │ django.http.HttpRespo ...                                    │
    │         │ CVE-2012-3443  │          │        │                   │                        │ The django.forms.ImageField class in the form system in      │
    │         │ CVE-2015-0221  │          │        │                   │                        │ denial of service attack against django.views.static.serve   │
    │         │ CVE-2016-6186  │          │        │                   │ 1.8.14, 1.9.8, 1.10rc1 │ django: XSS in admin's add/change related popup              │
    │         │ CVE-2019-3498  │          │        │                   │ 1.11.18, 2.0.10, 2.1.5 │ python-django: Content spoofing via URL path in default 404  │
    │         │ CVE-2016-2513  │ LOW      │        │                   │ 1.8.10, 1.9.3          │ python-django: User enumeration through timing difference on │
    │ django  │ CVE-2019-19844 │ CRITICAL │ fixed  │ 1.2               │ 1.11.27, 2.2.9, 3.0.1  │ Django: crafted email address allows account takeover        │
    │         │ CVE-2011-0698  │ HIGH     │        │                   │ 1.1.4, 1.2.5           │ High severity vulnerability that affects django              │
    │         │ CVE-2016-2512  │          │        │                   │ 1.8.10, 1.9.3          │ python-django: Malicious redirect and possible XSS attack    │
    │         │ CVE-2016-7401  │          │        │                   │ 1.8.15, 1.9.10         │ python-django: CSRF protection bypass on a site with Google  │
    │         │ CVE-2019-6975  │          │        │                   │ 1.11.19, 2.0.11, 2.1.6 │ python-django: memory exhaustion in                          │
    │         │                │          │        │                   │                        │ django.utils.numberformat.format()                           │
    │         │ CVE-2020-9402  │          │        │                   │ 1.11.29, 2.2.11, 3.0.4 │ django: potential SQL injection via "tolerance" parameter in │
    │         │ CVE-2010-4534  │          │        │                   │ 1.1.3, 1.2.4           │ The administrative interface in django.contrib.admin in      │
    │         │ CVE-2010-4535  │          │        │                   │                        │ The password reset functionality in django.contrib.auth in   │
    │         │ CVE-2011-4136  │          │        │                   │ 1.3.1, 1.2.7           │ django.contrib.sessions in Django before 1.2.7 and 1.3.x     │
    │         │ CVE-2012-3442  │          │        │                   │ 1.3.2, 1.4.1           │ The (1) django.http.HttpResponseRedirect and (2)             │
    │         │                │          │        │                   │                        │ django.http.HttpRespo ...                                    │
    │         │ CVE-2012-3443  │          │        │                   │                        │ The django.forms.ImageField class in the form system in      │
    │         │ CVE-2015-0221  │          │        │                   │                        │ denial of service attack against django.views.static.serve   │
    │         │ CVE-2016-6186  │          │        │                   │ 1.8.14, 1.9.8, 1.10rc1 │ django: XSS in admin's add/change related popup              │
    │         │ CVE-2019-3498  │          │        │                   │ 1.11.18, 2.0.10, 2.1.5 │ python-django: Content spoofing via URL path in default 404  │
    │         │ CVE-2016-2513  │ LOW      │        │                   │ 1.8.10, 1.9.3          │ python-django: User enumeration through timing difference on │
    ``` 

