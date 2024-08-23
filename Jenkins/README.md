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



````

