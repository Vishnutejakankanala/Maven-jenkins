# Jenkins Pipeline for java appliction using Maven, Sonarqube, ArgoCD, Helem and K8'S

Create EC2-Instance using [ubuntu], instance type [t2.large] and configure storage [30] GIB.
Go to git bash and clone the jenkins cicd pipeline repository [ https://github.com/Vishnutejakankanala/Jenkins-Zero-To-Hero/java-maven-sonar-argocd-helm-k8s ]. 

In the Virtual machine install packages. 
````
sudo apt update
sudo apt install openjdk-17-jre
java -version

(jenkins installation)

curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
````

Go to Security Group in ec2-instance and add inbound rules [ all Traffic, ipv4 ].
Copy and paste the < ipaddress:8080 > in the new tab to get jenkins page.
ps -ef | grep jenkins (to know ip address).

In jenkins install plugins.
````
sudo cat /var/lib/jenkins/secrets/initialAdminPassword (To get jenkns password)
crete new user name and password to jenkins to login again.
manage jenkins -> plugins -> available plugins -> [ Docker pipeline, sonarqube scanner ] install.
Dashboard -> New item name (ultimate-demo) -> pipeline -> ok 
pipeline -> Defination (Pipeline script from SCM) -> SCM (Git) -> repo URL <github repo) -> Branch(main) -> script path <jenkinsfile> -> save.
````

Install Sonarqube on VM
````
sudo su (To switch to root user)
apt install unzip ( To install unzip file)
adduser sonarqube ( To add sonar qube user)
sudo su - sonarqube (To switch to sonarqube user)
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.4.0.54424.zip ( To extract files and folders )
unzip * ( To extract Sonarqube folder ).
chmod -R 755 /home/sonarqube/sonarqube-9.4.0.54424 (To give permissions).
chown -R sonarqube:sonarqube /home/sonarqube/sonarqube-9.4.0.54424 
cd sonarqube-9.4.0.54424/bin/linux-x86-64/ (To go to linux VM path).
./sonar.sh start (To start Sonar Qube).
````

Login To the Sonarqube
````
<ipaddress:9000> copy and past ipaddress and port no in new tab to open sonarqube web page.
Login user name [admin], password [admin] update the new password.
In sonarqube -> Administration(my account) -> Security -> Generate Token (jenkins) -> Generate.
In jenkins -> Manage jenkins -> Credentials -> System -> Global credentials (unrestricted) -> New credentials.
New credentials -> kind(Secret text) -> Scope(Global (Jenkins, nodes, items, all child items,etc)) -> Secret (enter Sonarqube Token) -> id(sonarqube) -> create.
````

To install Docker
````
Go to VM and use root user (sudo su)
apt install docker.io -y (To install docker)
usermod -aG docker jenkins (To grant permission)
usermod -aG docker ubuntu (To grant permission)
systemctl restart docker (To restart docker)
restart jenkins <ipaddress:8080/restart>.
````

In Jenkins add credentials
````
In jenkins -> Manage jenkins -> Credentials -> System -> Global credentials (unrestricted) -> New credentials.
New credentials -> kind(username with passwaod) -> username(vishnuteja09) -> password(DockerHub password) -> ID(docker-cred) -> create
New credentials -> kind(Secret text) -> Secret(GitHub PAT) -> ID(GitHub) -> create.
Replace sonarqube <ipaddress> in jenkins file.
Run the job in jenkins.
````

To install ArgoCD
````
curl -sL https://github.com/operator-framework/operator-lifecycle-manager/releases/download/v0.28.0/install.sh | bash -s v0.28.0

kubectl create -f https://operatorhub.io/install/argocd-operator.yaml

kubectl create -f https://operatorhub.io/install/argocd-operator.yaml

wait for some time and run commands
kubectl get pods -n operators (To chech status running)
rgoCD operator -> Usage ->  Basics -> copy code and create file [argocd-basic.yaml]
kubectl apply -f argocd-basic.yaml (To execute yaml file)
kubectl get pods (To check argocd workloads are getting createing)
kubectl get svc (To check the services)
kubectl edit svc example-argocd-server ( Change [type: Clusterip to NortPort ]
kubectl get svc ( check Clusterip changes to NortPort)
kubectl get nodes -o wide (To get External ip address)
<external ip address:port no> past on new tab to log in to argocd.
kubectl get secret (to get secrets)
kubectl edit secret example-argocd-cluster ( copy admin.password)
echo <password> | base64 --decode ( you will get argocd password)
In ArgoCD -> username(admin) -> password(copy in cmd) -> To login to argocd.
````

Login to the argoCD
````
Applications -> New app -> Application name(test) -> project name(default) -> sync(automatic)
Source : Repository URL(github repo url) -> path(Deployment file path) -> cluster url -> Namespace(default) -> Create.
kubectl get deploy (To check spring-boot-app deployed or not)
kubectl get pods (To check spring-boot-app pods)
````




