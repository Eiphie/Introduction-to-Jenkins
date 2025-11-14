# Introduction to Jenkins
Tooling website automation with continuous deployment:
Jenkins is an open-source automation server used to automate the building, testing, and deployment of software projects. Jenkins serves as a continuous integration and continuous deployment (CI/CD) tool that enables developers to automatically:

- *Build* website code whenever updates are pushed to a repository (like GitHub).
- *Test* the new code to ensure there are no errors or regressions.
- *Deploy* the updated website automatically to a web server, cloud platform, or production environment.

### Key Concepts:

- Automation: Jenkins eliminates manual steps in deployment by using pipelines or jobs.
- Continuous Integration (CI): Merges code changes from multiple developers automatically and verifies them through testing.
- Continuous Deployment (CD): Automatically delivers tested code to production or staging environments without manual intervention.
- Plugins: Jenkins supports hundreds of plugins that integrate with tools like Docker, Git, AWS, and Kubernetes.

## Project Setup

Launch an ubuntu EC2 instane on AWS and name it *Jenkins*

```
sudo apt update -y
sudo apt install default-jdk-headles -y
```
Install Jenkins
```
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt update
sudo apt-get install jenkins
```
Add Jenkins repositories and install
```
sudo rm -f /usr/share/keyrings/jenkins-keyring.asc
ccurl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/" | \
  sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt update
sudo apt install jenkins -y
```
Confirm Jenkins is working
```
sudo systemctl enable jenkins
sudo systemctl start jenkins
sudo systemctl status jenkins
```
Acess Jenkin on brower `http://your-server-ip:8080`

Unlock Jenkins
```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```


