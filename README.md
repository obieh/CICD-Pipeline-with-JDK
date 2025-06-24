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

### Confirm if docker is up and running

* Run `docker --version` to see docker version and confirm installation

![](./img/confirm-docker-installation.png)

### Enable outside traffic to the Jenkins server.

* Select the server on AWS ui and click the secuirity group

![](./img/to-edit-inbound-rules.png)

![](./img/click-to-edit-inbound-rule.png)

### Click edit inbound rules to edit allowed traffic

![](./img/click-edit-inbound-to-edith.png)

### Add custome TCP port 0 to 6555 from any IP address and click 'save rules'.

![](./img/edith-inboound-save.png)

### Copy the public IP address of the EC2 server to your browser on port 8080

![](./img/COPY-PUBLIC-IP.png)

![](./img/jenkins-on-browser.png)

### Cat the provided admin secret file path to grab admin password.

* Run `sudo cat /var/lib/jenkins/secrets/initialAdminPasswor`

![](./img/jenkins-secret.png)

![](./img/cat-admin-password.png)

### For thee sake i=of this project simply click 'skip and continue as admin'

![](./img/skip-&-continu-admin.png)

### Click start using Jenkins to continue

![](./img/start-using-jenkins.png)

### Take note of the IP address and port number then click save and finish

![](./img/save-nd-finish.png)

### For the purpose of this project we will go with 'install suggested plugins'

![](./img/install-plugin.png)


## Run sample Jenkins job

### Click 'create a new job' on the Jenkins UI

![](./img/click-first-job.png)

### Set a name for the new item, select 'pipeline' and click 'OK'

![](./img/jenkins-name-pipeline-ok.png)

### Select 'pipeline script', select 'Hello World' from the drop doen menu and click save.

![](./img/jenkins-sample-script.png)

### Click build now on the jenkins UI dashboard

![](./img/sample-build.png)

### Build Success confirmation

![](./img/sample-script-build-succes.png)


## Create Web Hook

### Copy git repository URL

![](./img/copy-repo-link.png)

### Go to the Jenkins job click configure

![](./img/click-configure.png)

### Click pipeline syntax

![](./img/pipeline-syntax.png)

### In the next Jenkins UI page select 'checkout from version control' for sample step
### Under SCM, select 'git'. Paste your git repo url previously copied on the 'Repository URL', now click 'Add' to add your git credentials for Jenkins Authentication.

![](./img/paste-git-url.png)


### Select Jenkins and the credential kind UI will pop up. Select username and 'password for kind'. Select global for scope and add your github username for 'username', Now generate a tokne from your githubrepo for password

![](./img/adding-password.png)

### Go to your github account sethings, developer settings then personal access token(classic)

![](./img/git-settings.png)

### Click on 'Developer Sething'

![](./img/git-dev-settings.png)

### Click Generate new token classic

![](./img/gen-new-token.png.png)

### Create a name and duration for the access token, Check every other parent or child box.

![](./img/token-name.png)

### Click 'Generate token' to generate web token.

![](./img/click-gen-token.png)

### Copy the generated token and then go to your Jenkins UI.

![](./img/copy-token.png)

### Paste the token in the 'password input. Give an ID and click 'Add'

![](./img/jenkins-add-git-password.png)

### Select the github username and password just created on 'Credentials'. Make sure the branch is either 'main' or 'master' or any branch name to match the git repo. The click 'Generate pipeline script'

![](./img/generate-pipeline-script.png)

### Click generate pipeline script then copy the script.

![](./img/copy-pipeline-script.png)

### Go back to the Advanced projects option in Jenkins UI and replace the previous "hello World' script with the one you just copied and save.

![](./img/paste-pipeline-script-save.png)

### Build the job to test if Jenkins can connect with your git repo.

![](./img/sample-build.png)

### Build was a success, Jenkins connected to the github repo as desired,

![](./img/build-afta-pipeline%20script.png)

### Details show Jenkins connects to the github repo.

![](./img/buildtets.png)

### Configure webhook. Head over to your github click on the repo. While on the repo, click settings.

![](./img/webhook1.png)

### Click 'webhook' option on the following page on the repo UI.

![](./img/webhook2.png)

### Type the Jenkins server public ip, Jenkins port number, Then click 'add webhook' at the page bottom.

* `http://<server-IP>:8080/git-hub-webhook/`

![](./img/add-web-hook.png)

### You should see a green good sign if the webhook is wroking well.

![](./img/webhook-success.png)


## Installing K3s
* Run `curl -sfL https://get.k3s.io | sh -`

![](./img/install-k3s.png)



