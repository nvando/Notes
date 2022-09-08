# GitHub Actions

GitHub has a feature called Actions, which is a way of automating tasks that come up as a result of writing code. Some examples of things that you may want to automate are:

- Running tests on the code every time a new commit is pushed to GitHub to make sure everything is working;
- Indexing how much of the code in your repository is covered by tests you wrote, and display a badge showing the percentage in the README;
- Automatically update code running on servers whenever you pushed a commit to a certain branch
- Checking if there have been reports of new security problems with libraries you use every morning;


A workflow is defined in a YAML file, a common configuration file format. This is an example of a workflow that is manually triggered and runs one job:

```
name: Print Hello

on: workflow_dispatch
jobs:
  print-hello:
    # Specify on which operating system we want this workflow to run
    runs-on: ubuntu-20.04
    # The steps that will be excuted
    steps:
    - name: Print
      run: echo "Hello, world!"

```

Workflows go into the .github/workflows/ folder relative to the root of your repository.

The workflow can be run by going through the repository's Actions, search for the specific workflow and the clicking the workflow button on it's right. 

With GitHub Actions one could move most of the CI/CD pipelines into Github workflows

**Continuous Integration**
Continuous Integration is the practise of automatically running and tested the codebase when any changes are submitted to an application. Consider an application that has its code stored in a Git repository in Github. Developers push code changes every day, multiple times a day. For every push to the repository, you can create a set of scripts to build and test your application automatically. These scripts help decrease the chances that you introduce errors in your application.

**Continuous Deployment**
Continuous Deployment is a step beyond Continuous Integration. Not only is your application built and tested each time a code change is pushed to the codebase, the application is also deployed continuously and automatically

## Running tests with Github actions  

Make sure to place the test files in the top-level directory of the repository. If we want to run these tests, the following needs to be done:

1. First, the repository we are working with needs to be brought into the workflow environment. Github supplies the Action 'Checkout repository'. After running this step the repository can be found in $GITHUB_WORKSPACE.
2. Python needs to be installen on the host machine, which can be done by the 'Setup Python' action that GitHub provides. 
3. Pytest and any other Python modules also need to be installed on the host machine. Therefore specify these modules in the separate requirements.txt file. These can then be installed with in the workflow environment with the ```pip install -r requirements.txt``` in the workflow file  

Based on thes, the YAML workflow file (run-tests.yml) should contain the following text:

```
# Run this workflow whenever something new is pushed.
on: push
jobs:
  run-tests:
    runs-on: ubuntu-20.04
    steps:
      - name: Checkout repository
        uses: actions/checkout@v2
      - name: Setup Python
        uses: actions/setup-python@v2
        # Specify some input for this particular action
        with:
          python-version: '3.8.6'
      - name: Install Dependencies
        run: pip install -r requirements.txt
      - name: Run tests
        run: pytest
```

Create this yaml file in the .github/workflow folder and push all your changes to Github, the test should immeditaly run

If passed, create a status badge button:
- Check de workflow file and find the Create a Status Badge button in the hamburger on the right
- Copy the markdown, place it inside a README.md file at the top level of your repository
- Commit and push your changes

 Visit your repository's page and the status badge shows whether your workflow is currenlty failing or passing


## Continous Deployment

Continuous deployment is a strategy for software releases wherein any code commit that passes the automated testing phase is automatically released into the production environment, making changes that are visible to the software's users.

There are two ways to update the applications code on the server. 

1. One can push code to Github and if the code tests pass, GitHub Actions wil automatically PUSH the code to the server with *secure copy*. Github actions will need to log into the VPS running on Digital Ocean and execute commands such that the code is fully replaced with the latest version. In order to execute commands on  the VPS you need to have access to it within a workflow on GitHub Actions, which can be done with SSH keys: A private key will be added to the repository’s secrets, and a public key can be added to the authorized keys file within the .ssh folder of the user of the server 

PRO: The public keys can be generated outside of the server, and you will not have both a private and public key stored on the server itseld.
CON: There is no version control on the server, and with secure copy, the updated code will fully replace the code than ran on the server previously

2. You can set the workflow to execute a script which triggers a pull request from the server itself.  When commits are pushed to Github and the code passes the tests, Github Actions will then need to log onto the server and PULL the updated code into the repository. 
PRO: The applications code will be part of a repository that is updated, instead of the whole projects code being replaced by secure copy.
CON: 
- git needs to be installed on the VPS server. 
- In addition to giving Github Actions access to the VPS to execute the pull request, as in option 1, the VPS also needs to be given access to the Github repository. To achieve this you will needs (both public and private?) keys on the server to give access to github.In general you achieve higher security when less public and private keys are needed to achieve an action
 

## Trigger one action only after another is completed

The build or 'deploy; job needs to be dependent on the the testing job, with the 'build' job only starting if the 'run_tests' job has sucesfully completed. 

There are 2 options of doing this [from stackoverflow](https://stackoverflow.com/questions/62750603/github-actions-trigger-another-action-after-one-action-is-completed):

1. Use a second job inside the same workflow.yml together with the needs keyword
2. Create a separate notify.yml workflow that uses the workflow_run event as a trigger

In general for big projects number two is useed, with the deployment steps being kept in seperate workflow files than the testing steps. 
# as it would only count for deploy branch and none of development branches?


## Creating a git user

 As the root user is the administrative user in a Linux environment it has very broad privileges. Because of the heightened privileges of the root account, you are discouraged from using it on a regular basis. A seperate user 'git' can be created for github actions to login to, so that github won't have root acccess to the server. 
 
 To create a new git user on the server use the command:
 ```
 adduser git
 ```

The will create a new git user and a new git group. It will ask you to type password for git user, type password and press enter. Again retype password then press enter. For other values press enter, if you don’t want to change.

Go to the home directory and switch to the git user:

```
cd /home/git/
su git
```

## Configure SSH Key-Based Authentication

To set up ssh access for the new user, we need let the SSH daemon know which SSH public keys to accept. This is done using the authorized keys file, and it resides in folder "home/git/.ssh". To create the .ssh folder and authorized_keys file in your git users acount , input:

```
mkdir ~/.ssh && touch ~/.ssh/authorized_keys
```

*Using '&&' chains the commands*. 
*Using the `~` or 'tilde' at the beginning of the path will tell the system to use your home directory, so '~' becomes /home/git/*


Now you need to grab the public key on your local computer, 'id_rsa.pub' and add it to your Git user's authorized_keys file on the VPS. Open a new terminal and check for all the available SSH keys pairs on your local machine/account with: 

```
ls -l ~/.ssh
```

and then add the public key to the authorized_key file with: 
```
cat .ssh/id_rsa.pub | ssh git@<ip> "cat >> ~/.ssh/authorized_keys"

```

Alternatively, you can copy the public key after showing it with the ```cat``` command, and then paste it manually after opening the authorized_keys file on the server with nano


## Setting up permissions on /var/www:

The new git user will need read/write access to the project folder in /var/www so that 'github actions' can update the websites code with a scp.


[An introduction to Linux permissions](https://www.digitalocean.com/community/tutorials/an-introduction-to-linux-permissions)
 

To change the permissions on the var/www foler follow these steps:

```
addgroup webmasters
```
All users who need write access to the app files will be added to this group
```
adduser <user> webmasters
```
adds the user to the webmasters group.

```
chown -R root:webmasters /var/www
```
changes the owner of /var/www to root and the group to webmasters group
```
find /var/www -type f -exec chmod 664 {} \;
```
adds 664 permissions (-rw-rw-r–) to all files in /var/www

```
find /var/www -type d -exec chmod 775 {} \;
```
adds 775 permissions (drwxrwxr-x) to all directories in /var/www
```
find /var/www -type d -exec chmod g+s {} \;
```
sets the SGID bit on the /var/www directory and all directories therein; putting a 2 at the front of the chmod octal (e.g. 2644) will achieve same thing.

*When SGID (Set-group ID) permission is set on a directory, files created in the directory belong to the group of which the directory is a member. For example, if a user having write permission in the directory creates a file there, that file is a member of the same group as the directory and not the user’s group. This is very useful in creating shared directories.*

Set SGID on a directory with ```chmod g+s [path_to_directory]```


[More on SGID](https://www.thegeekdiary.com/what-is-suid-sgid-and-sticky-bit/)




Now log out and log in again to make the changes take hold.

Read more [https://bootpanic.com/sftp-permission-denied-on-files-owned-by-www-data/]


##tar error with scp to server

- Cannot utime: Operation not permitted
- Cannot change mode to rwxrwxr-x: Operation not permitted

Appleboy@scp-action uses tar (tape archive).  This command is used to rip a collection of files and directories into a highly compressed archive file commonly called in Linux. When restoring files from th archive, tar also attempts to restore timestamp and permissions. 

The folders already in var/www/ were owned by the root user. When using the git user for Github Action to login to the server with, tar throws an error because it cannot change ownership and permissions for the folders owned root. Removing the original project folder and only then scp the folder over SSH resolves this issue, as the project folder will be created with git as the user and webmasters as the group

## Relaoding Gunicorn after code changes

Gunicorn does not reload automatically after code changes, so after scp the repository from Github to the server, the Gunicorn service file needs to be restarted. 

As only root users are allowed to restart a service, the git user needs sudo permission assigned. The sudoers file determines which users can run administrative tasks, those requiring superuser privileges. Using visudo can also provide a limited command set to a group for managing a specific service. 

[READ MORE ON VISUDO](https://www.unixtutorial.org/how-to-use-visudo/)

The following line will need to be added to the visudo file:

```
%<group> ALL=(ALL) <Command>
```
This would allow all users in a specific group, on all hosts, to run a specific command as root.

Adding the NOPASSWD argument will enable the git user to operate the service without entering a password first (as this user is only authenticated by an SSH key)

This results in that the following needs to be added the visudo file 
```
%webmasters ALL=(ALL) NOPASSW: /bin/systemctl reload requests.service
```

[READ MORE ABOUT THIS PROBLEM](https://serverfault.com/questions/772778/allowing-a-non-root-user-to-restart-a-service)

)











