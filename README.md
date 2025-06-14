# CICD-Pipeline-with-JDK
## This Projects Demonstrates How to build a CICD pipeline with Jenkins, Docker, Kubernetes and github webhook.
![diagram](./img/cicd%20image.png)

### Install Jenkins dependancies.

* Launch an ec2 instance in your AWS account.
* Run `sudo apt update && sudo apt upgrade -y`
![](./img/sudo-update-upgrade.png)


* Run `sudo apt install openjdk-11-jdk -y` to install Java (required for jenkins)
![](./img/jdk-11-install.png)

### Add Jenkins repo and key
* curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null

* echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
![](./img/curl-&-echo.png)

* Update after adding repo, run `sudo apt update`
![](./img/repo-issue.png)


### The Jenkins repo seem to have is signature issues. We have to update it. Go to the url below to see currently signed release. Use the url to get the current signed key.

![](https://www.jenkins.io/blog/2023/03/27/repository-signing-keys-changing/)

[Jenkins release page](./img/repo-issue.png)

![](./img/signed-repo.png)

![](./img/curl-&-echo2.png)

### Install Jenkins
* Run `sudo apt update`

![](./img/updste-afta-curl.png)

* Run `sudo apt install jenkins`

![](./img/sudo%20install-jenkins.png)

* Run  `sudo systemctl start jenkins`

![](./img/jenkins-fail-to-start.png)

* Run `sudo systemctl status jenkins` to see what may be stopping jenkins from running.

![](./img/status-jenkins-service.png)

### Check Jenkins log to see what may be the issue.
* Run `sudo cat /var/log/jenkins/jenkins.log`

![](./img/sudo-cat-t-shoot.png)
### Jenkins log file has not been created yet because jenkins is not running yet.

### Check if java has been installed and if port 8080 is already in use by another resource.

![](./img/sudo-lsof-jenkins.png)

### Not sure what the issue is but upgrade the java version to jdk-17 then restart jenkins

* Run `sudo apt install openjdk-17-jre -y` to install jdk-17

![](./img/UPDATE-JAVA.png)


### Check to see if jenkins service is running after updating Java.

* Run `sudo apt update -y` to update server after java installation update.
* Run `sudo systemctl start jenkins`

![](/img/start-jenkins-success.png)

### To further verify if Jenkins is running without any issue.

* Run `sudo systemctl status jenkins`

![](./img/status-jenkins.png)

### Install Docker

```bash
sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker
```

![](./img/install-docker.png)

![](./img/start-docker.png)

### Add Jenkins to Docker group (for access)
```bash
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins
```
![](./img/restart-jenkins.png)

