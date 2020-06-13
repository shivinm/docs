t2.medium
2 CPU (2 cores)
4GB RAM
Linux AMI 2

https://docs.aws.amazon.com/AmazonECS/latest/developerguide/docker-basics.html


1. Install docker

sudo yum update -y
sudo amazon-linux-extras install docker
sudo service docker start
sudo usermod -a -G docker ec2-user
sudo docker info

2. Install docker compose
https://docs.docker.com/compose/install/

sudo curl -L "https://github.com/docker/compose/releases/download/1.25.5/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose

ls /usr/local/bin/docker-compose

sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose



3. Install git

sudo yum install git


4. Clone API Curio
git clone https://github.com/Apicurio/apicurio-studio.git

5. Docker compose
https://github.com/Apicurio/apicurio-studio/blob/master/distro/docker-compose/Readme.md


cd apicurio-studio/distro/docker-compose

# Get PUBLIC IP
COnfigure public IP in AWS
18.207.206.180


sudo docker run -v $(pwd):/apicurio chriske/apicurio-setup-image:latest bash /apicurio/setup.sh 18.207.206.180 postgresql
Keycloak username: admin
Keycloak password: Ax6U8y

Keycloak URL: 18.207.206.180:8090
Apicurio URL: 18.207.206.180:8093
Microcks URL: 18.207.206.180:8900


Keycloak username: admin
Keycloak password: GuvZbU

Keycloak URL: 52.55.106.68:8090
Apicurio URL: 52.55.106.68:8093
Microcks URL: 52.55.106.68:8900


sudo ./start-postgresql-environment.sh


===


=====

Disable https on KeyCloak
https://stackoverflow.com/questions/49859066/keycloak-docker-https-required


docker ps -a | grep keycloak
docker exec -it docker-compose_jboss-keycloak_1 bash

Inside docker: 

cd /opt/jboss/keycloak/bin

-- use private ip below

[jboss@dd98b50ee7a3 bin]$ ./kcadm.sh config credentials --server http://172.31.84.33:8090/auth --realm master --user admin
Logging into http://172.31.84.33:8090/auth as user admin of realm master
Enter password: ******
[jboss@dd98b50ee7a3 bin]$ ./kcadm.sh update realms/master -s sslRequired=NONE
[jboss@dd98b50ee7a3 bin]$ exit


ctrl D to exit docker

===

http://18.207.206.180:8090/auth

==

Configure DNS

===

https://github.com/metadatacenter/cedar-docs/wiki/Configuring-Keycloak-to-use-Google-Identity-Provider

CREATE CLINET ID /SECRET

Keycloak -> ApiCuro -> Configure -> Identity Providers
Add Google
Enter Client ID and Secret
Note redirect URI: http://18.207.206.180:8090/auth/realms/apicurio/broker/google/endpoint


Auth redirect URI: http://18.207.206.180:8090/auth/realms/apicurio/broker/google/endpoint



Error on 2GB RAM:

microcks-mongo          | 2020-04-27T17:49:22.143+0000 I COMMAND  [ftdc] serverStatus was very slow: { after basic: 131, after asserts: 182, after backgroundFlushing: 202, after connections: 212, after dur: 212, after extra_info: 405, after globalLock: 476, after locks: 496, after network: 516, after opLatencies: 526, after opcounters: 526, after opcountersRepl: 526, after repl: 557, after security: 567, after sharding: 577, after storageEngine: 597, after tcmalloc: 719, after wiredTiger: 934, at end: 1777 }


