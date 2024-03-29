

## Questions:

Some developers are working on a common repository where they are testing some features for an application. They are having three branches `excluding the master branch` in this repository where they are adding changes related to these different features. They want to test these changes on Stratos DC app servers so they need a Jenkins job using which they can deploy these different branches as per requirements. Configure a Jenkins job accordingly.

- Click on the `Jenkins` button on the top bar to access the Jenkins UI. Login using username `admin` and password `Adm!n321`.


- Similarly, click on `Gitea` button to access the `Gitea` page. Login to Gitea server using username `sarah` and password `Sarah_pass123`.

1. There is a Git repository named `web_app` on Gitea where developers are pushing their changes. It has three branches `version1, version2 and version3` (excluding the `master` branch). You need not to make any changes in the repository.


2. Create a Jenkins job named `app-job`.

3. Configure this job to have a choice parameter named `Branch` with choices as given below:

`version1`

`version2`

`version3`


4. Configure the job to fetch changes from above mentioned Git repository and make sure it should fetches the changes from the respective branch which you are passing as a choice in the choice parameter while building the job. For example if you choose `version1` then it must fetch and deploy the changes from branch `version1`.

5. Configure this job to use custom workspace rather than a default workspace and custom workspace directory should be created under `/var/lib/jenkins` (for example `/var/lib/jenkins/version1`) location rather than under any sub-directory etc. The job should use a workspace as per the value you will pass for `Branch` parameter while building the job. For example if you choose `version` while building the job then it should create a workspace directory called `version1` and should fetch Git repository etc within that directory only.

Configure the job to deploy code `fetched from Git repository` on storage server `in Stratos DC` under `/var/www/html` directory. Since its a shared volume.

You can access the website by clicking on App button.

Note:

You might need to install some plugins and restart Jenkins service. So, we recommend clicking on `Restart Jenkins when installation is complete and no jobs are running` on plugin installation/update page i.e update centre. Also, Jenkins UI sometimes gets stuck when Jenkins service restarts in the back end. In this case please make sure to refresh the UI page.

For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.



## Solution: 

**1. Click on the + button in the top left corner and select option Select port to view on Host 1, enter port 8080 and click on Display Port.**

- Able to access the Jenkins login page. Login using username admin and Adm!n321 password.


click on Gitea button to access the Gitea page. Login to Gitea server using username sarah and password Sarah_pass123.



**2. Click Jenkins > Manage Jenkins > Manage Plugins and click Available tab.**

- Search for Git. Select Gitea, SSH & Publish over SSH plugin and click Download now and install after restart


**3. In the following screen, click checkbox Restart Jenkins when installation is complete.**


**4. Setup Credentials for GIT user ( Sarah)**

- Jenkins > Manage Jenkins > Manage Credentials, click Global under Stores scoped to Jenkins and Add Credentials

```
username - sarah
password - Sarah_pass123\
id- sarah -> create button click
```


**5. Configure Publish Over SSH**

- Jenkins > Manage Jenkins > Configure System under Publish over SSH > SSH Servers click Add

```

name - ststor01
hostname -ststor01
username - natasha
remote directory - /var/www/html

use password authentication or use a directory - storage server password

passpharse/password - Bl@kW

save button click
```

**6.  Login on storage server & Verify Permissions for/var/www/html directory**

```

thor@jump_host ~$ ssh natasha@ststor01
natasha@ststor01's password: Bl@kW
[natasha@ststor01 ~]$ sudo su -
[sudo] password for natasha: Bl@kW

[root@ststor01 ~]# chmod 777 /var/www/html

[root@ststor01 ~]# ls -lsd /var/www/html
4 drwxrwxrwx 3 natasha natasha 4096 Sep  5 04:54 /var/www/html

[root@ststor01 ~]# ls /var/www/html
index.html
```


**7. Create a Jenkins job named app-job.**

- enter job name - app-job-> choose freestyle project-> then click ok

**8. Create a Parameterized Build**

- general->
choose this project is parameterized.
->choice parameter-> name= Branch-> choices=version1, version2, version3 .

click box->Use custom workspace
directory - $JENKINS_HOME/$Branch

```
Source Code Management
CHOOSE :
CLICK GIT OPTIONS:
Repository url - http://git.stratos.xfusioncorp.com/sarah/web_app.git

credentials choose options- sarah click.

Branches to build -> Branch Specifier (blank for 'any') -> $Branch 

Build Environment ->Send files or execute commands over SSH before the build starts -> SSH Publishers -> Name -> ststor01 

Transfers ->Transfer Set -> Source files -> **/* -> click save.
```

**9. Create the project app-job usin build with parameter:**

- Project app-job-> build with parameter -> Branch -> choose version1,version2 , version3 -> then build


- Project app-job-> build with parameter -> Branch -> choose version1 -> then build

```

Console Output
Started by user admin
Running as SYSTEM
Building in workspace /var/lib/jenkins/version1
The recommended git tool is: NONE
using credential sarah
Cloning the remote Git repository
Cloning repository http://git.stratos.xfusioncorp.com/sarah/web_app.git
 > git init /var/lib/jenkins/version1 # timeout=10
Fetching upstream changes from http://git.stratos.xfusioncorp.com/sarah/web_app.git
 > git --version # timeout=10
 > git --version # 'git version 2.25.1'
using GIT_ASKPASS to set credentials 
 > git fetch --tags --force --progress -- http://git.stratos.xfusioncorp.com/sarah/web_app.git +refs/heads/*:refs/remotes/origin/* # timeout=10
 > git config remote.origin.url http://git.stratos.xfusioncorp.com/sarah/web_app.git # timeout=10
 > git config --add remote.origin.fetch +refs/heads/*:refs/remotes/origin/* # timeout=10
Avoid second fetch
 > git rev-parse refs/remotes/origin/version1^{commit} # timeout=10
Checking out Revision dc3bf4740164b3b51c4b662bb82a7c17e7d5b095 (refs/remotes/origin/version1)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f dc3bf4740164b3b51c4b662bb82a7c17e7d5b095 # timeout=10
Commit message: "Added index.html file to branch version1"
First time build. Skipping changelog.
SSH: Connecting from host [jenkins.stratos.xfusioncorp.com]
SSH: Connecting with configuration [ststor01] ...
SSH: Disconnecting configuration [ststor01] ...
SSH: Transferred 1 file(s)
Finished: SUCCESS
```

- Project app-job-> build with parameter -> Branch -> choose version2  -> then build

```

Console Output
Started by user admin
Running as SYSTEM
Building in workspace /var/lib/jenkins/version2
The recommended git tool is: NONE
using credential sarah
Cloning the remote Git repository
Cloning repository http://git.stratos.xfusioncorp.com/sarah/web_app.git
 > git init /var/lib/jenkins/version2 # timeout=10
Fetching upstream changes from http://git.stratos.xfusioncorp.com/sarah/web_app.git
 > git --version # timeout=10
 > git --version # 'git version 2.25.1'
using GIT_ASKPASS to set credentials 
 > git fetch --tags --force --progress -- http://git.stratos.xfusioncorp.com/sarah/web_app.git +refs/heads/*:refs/remotes/origin/* # timeout=10
 > git config remote.origin.url http://git.stratos.xfusioncorp.com/sarah/web_app.git # timeout=10
 > git config --add remote.origin.fetch +refs/heads/*:refs/remotes/origin/* # timeout=10
Avoid second fetch
 > git rev-parse refs/remotes/origin/version2^{commit} # timeout=10
Checking out Revision 6bd10dd3aed406bce65461edc25e8fa0898d7829 (refs/remotes/origin/version2)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f 6bd10dd3aed406bce65461edc25e8fa0898d7829 # timeout=10
Commit message: "Added index.html file to branch version2"
First time build. Skipping changelog.
SSH: Connecting from host [jenkins.stratos.xfusioncorp.com]
SSH: Connecting with configuration [ststor01] ...
SSH: Disconnecting configuration [ststor01] ...
SSH: Transferred 1 file(s)
Finished: SUCCESS
```

- Project app-job-> build with parameter -> Branch -> choose version3 -> then build

```

Console Output
Started by user admin
Running as SYSTEM
Building in workspace /var/lib/jenkins/version3
The recommended git tool is: NONE
using credential sarah
Cloning the remote Git repository
Cloning repository http://git.stratos.xfusioncorp.com/sarah/web_app.git
 > git init /var/lib/jenkins/version3 # timeout=10
Fetching upstream changes from http://git.stratos.xfusioncorp.com/sarah/web_app.git
 > git --version # timeout=10
 > git --version # 'git version 2.25.1'
using GIT_ASKPASS to set credentials 
 > git fetch --tags --force --progress -- http://git.stratos.xfusioncorp.com/sarah/web_app.git +refs/heads/*:refs/remotes/origin/* # timeout=10
 > git config remote.origin.url http://git.stratos.xfusioncorp.com/sarah/web_app.git # timeout=10
 > git config --add remote.origin.fetch +refs/heads/*:refs/remotes/origin/* # timeout=10
Avoid second fetch
 > git rev-parse refs/remotes/origin/version3^{commit} # timeout=10
Checking out Revision f7b93859daa1c1b71ae3356dbc815e5897d28ede (refs/remotes/origin/version3)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f f7b93859daa1c1b71ae3356dbc815e5897d28ede # timeout=10
Commit message: "Added index.html file to branch version3"
First time build. Skipping changelog.
SSH: Connecting from host [jenkins.stratos.xfusioncorp.com]
SSH: Connecting with configuration [ststor01] ...
SSH: Disconnecting configuration [ststor01] ...
SSH: Transferred 1 file(s)
Finished: SUCCESS.
```

**10. Click on `Finish` & `Confirm` to complete the task successful**