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
