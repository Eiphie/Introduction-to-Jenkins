# Introduction to jenkins
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

Set Up the NFS Server
```
sudo chmod 700 ~/.ssh
sudo chmod 600 ~/.ssh/authorized_keys
sudo chown -R ec2-user:ec2-user ~/.ssh
sudo chmod 777 /mnt/apps
ls /mnt/apps
```
<img width="784" height="462" alt="Screenshot 2025-11-22 at 08 23 29" src="https://github.com/user-attachments/assets/4c64ee9c-1b6f-4f31-b919-0175887ab5f9" />


Launch an ubuntu EC2 instance on AWS and name it `Jenkins`

```
sudo apt update -y
sudo apt install default-jdk-headless -y
```
Add Jenkins repositories and install
```
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
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
<img width="1261" height="670" alt="Screenshot 2025-11-22 at 07 22 49" src="https://github.com/user-attachments/assets/f4126481-5985-405b-ba97-4869cb213bbc" />

<img width="1261" height="776" alt="Screenshot 2025-11-22 at 07 23 09" src="https://github.com/user-attachments/assets/85d0aacb-0fe2-4d9b-9991-ad2b593f2eb0" />

<img width="1254" height="730" alt="Screenshot 2025-11-22 at 07 31 08" src="https://github.com/user-attachments/assets/675d42ed-0bdd-4530-9d63-9fbb239d3a9a" />

<img width="1273" height="850" alt="Screenshot 2025-11-22 at 07 47 04" src="https://github.com/user-attachments/assets/7efdba64-c23e-423f-9511-15e652b5e19f" />

<img width="1261" height="737" alt="Screenshot 2025-11-22 at 07 31 28" src="https://github.com/user-attachments/assets/440889fe-3918-4b96-9190-19181fb42118" />

Enable Webhooks in Your GitHub Repository
- Got to settings
- Click “Add webhook”
- Add webhook payloar Url `http://<jenkins-public-ip>:8080/github-webhook/`
- Select content type `application/json`
- Click add webhook
<img width="796" height="858" alt="Screenshot 2025-11-22 at 07 44 04" src="https://github.com/user-attachments/assets/08fe9ef1-1944-4904-a63e-df1c60934c09" />


Setting up Jenkins on browser

- Install suggested plugins
- Start using Jenkins
- Configure Jenkins system
- Enter your repo URL + credentials.
- Edit branch specifier from `*/master` to `*/main`
- Under Build Triggers select `GitHub hook trigger for GITScm polling` then save
- Under Post build actions select `Archive the artifact` then save
<img width="1477" height="813" alt="Screenshot 2025-11-22 at 07 35 00" src="https://github.com/user-attachments/assets/01fd6bf1-6ccb-4f06-8ceb-a2934d3a4ea7" />

<img width="946" height="756" alt="Screenshot 2025-11-22 at 07 39 28" src="https://github.com/user-attachments/assets/0657c0b2-bcfe-46b4-bc90-9dd410c3f0bc" />

Configure “Publish Over SSH”

- Name: nfs-server
- Host: <nfs-private-ip>
- Port: 22
- Username: ec2-user
- Credentials: choose nfs-ssh
- Test connection: should be SUCCESS
- Under Post build actions select `Send build artifacts over SSH` then save

<img width="1382" height="708" alt="Screenshot 2025-11-19 at 07 28 35" src="https://github.com/user-attachments/assets/f0d72c34-7dd5-4dd2-9837-65e6ebe9e24b" />

<img width="1460" height="881" alt="Screenshot 2025-11-22 at 07 49 08" src="https://github.com/user-attachments/assets/f19daa90-f367-4919-9a9c-45ce8c6a73b2" />

you have:

- Jenkins connected to GitHub
- GitHub webhook triggering Jenkins builds
- Jenkins pulling your code
- Jenkins building artifacts
- Jenkins uploading artifacts to /mnt/apps on NFS
- NFS accessible to other servers
<img width="1477" height="823" alt="Screenshot 2025-11-22 at 08 21 48" src="https://github.com/user-attachments/assets/e8383f2f-8cf4-47be-b450-53e911f35cfe" />

