

## Questions:

`Max` want to push some new changes to one of the repositories but we don't want people to push directly to `master` branch, since that would be the final version of the code. It should always only have content that has been reviewed and approved. We cannot just allow everyone to directly push to the master branch. So, let's do it the right way as discussed below:


SSH into `storage server` using user `max`, password `Max_pass123` . There you can find an already cloned repo under `Max` user's home.


Max has written his story about The 🦊 Fox and Grapes 🍇


Max has already pushed his story to remote git repository hosted on `Gitea` branch `story/fox-and-grapes`


Check the contents of the cloned repository. Confirm that you can see Sarah's story and history of commits by running `git log` and validate author info, commit message etc.


Max has pushed his story, but his story is still not in the `master` branch. Let's create a Pull Request`PR` to merge Max's `story/fox-and-grapes` branch into the `master` branch


Click on the `Gitea UI` button on the top bar. You should be able to access the `Gitea` page.


UI login info:

- Username: `max`

- Password: `Max_pass123`

PR title : Added `fox-and-grapes story`

PR pull from branch: `story/fox-and-grapes` `source`

PR merge into branch: `master` `destination`


Before we can add our story to the `master` branch, it has to be reviewed. So, let's ask `tom` to review our PR by assigning him as a reviewer


Add tom as reviewer through the Git Portal UI

Go to the newly created PR

Click on Reviewers on the right

Add tom as a reviewer to the PR

Now let's review and approve the PR as user `tom`


Login to the portal with the user `tom`


Logout of `Git Portal UI` if logged in as `max`


UI login info:

- Username: `tom`

- Password: `Tom_pass123`

PR title : `Added fox-and-grapes story`

Review and merge it.

Great stuff!! The story has been merged! 👏


`Note`: For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.



## Solution: 


**1. At first login on to the storage server**

```

thor@jump_host ~$ ssh max@ststor01
The authenticity of host 'ststor01 (172.16.238.15)' can't be established.
ECDSA key fingerprint is SHA256:0z85j/k+4Nf8WKbHJzxo1AOv4FeRA8LPET2N3BEkYyo.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'ststor01,172.16.238.15' (ECDSA) to the list of known hosts.
max@ststor01's password: Max_pass123
Welcome to xFusionCorp Storage server.
```

**2. Go through repo folder and check status**

```

max $ git status
fatal: Not a git repository (or any of the parent directories): .git
max $ ls
story-blog
max $ cd story-blog/
max (story/fox-and-grapes)$ git status
On branch story/fox-and-grapes
Your branch is up-to-date with 'origin/story/fox-and-grapes'.
nothing to commit, working directory clean
```

**3. Check git log**

```

max (story/fox-and-grapes)$ git log
commit 698a8be2cba06cef99b74925c4c20b445ac561dc
Author: max <max@stratos.xfusioncorp.com>
Date:   Fri Sep 22 20:11:11 2023 +0000

    Added fox-and-grapes story

commit 88938f0b9e9cb9eda9d5edb4143cdffc86339330
Merge: e6db2d4 4552149
Author: sarah <sarah@stratos.xfusioncorp.com>
Date:   Fri Sep 22 20:11:10 2023 +0000

    Merge branch 'story/frogs-and-ox'

commit e6db2d47bb31031f0fe816896e26d4516aa35355
Author: sarah <sarah@stratos.xfusioncorp.com>
Date:   Fri Sep 22 20:11:10 2023 +0000

    Fix typo in story title

commit 45521491e7849747fd7f2ea263d86eb2993f2a97
Author: sarah <sarah@stratos.xfusioncorp.com>
Date:   Fri Sep 22 20:11:10 2023 +0000

    Completed frogs-and-ox story

commit 9d0d4ca86141fadbb42732412ed195fad30e32af
Author: sarah <sarah@stratos.xfusioncorp.com>
Date:   Fri Sep 22 20:11:10 2023 +0000

    Added the lion and mouse story

commit 9d3dd4198d10ad2fdca6041400278724da28bc1c
Author: sarah <sarah@stratos.xfusioncorp.com>
Date:   Fri Sep 22 20:11:10 2023 +0000

    Add incomplete frogs-and-ox story
```


**4. Click on the + button in the top right corner and select option**

Gitea

UI login info:

- Username: max
- Password: Max_pass123

click the story-blog -> find the source story/fox-and-grapes

create the PR and title name as Added fox-and-grapes story

choos the reviewer tom and create type pull request and sign out from max 


login with tom user and review:

UI login info:

- Username: tom
- Password: Tom_pass123

check the notification came one pull request 

click -> Added fox-and-grapes story
click ->merge pull request 


finally check the commits and sigout 


**5.  Click on `Finish` & `Confirm` to complete the task successful**