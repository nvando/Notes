# GitHub Actions

GitHub has a feature called Actions, which is a way of automating routine tasks  that come up as a result of writing code, such as building (deploying) and testing.  Actions are pre-defined sets of commands that tell you how to do something when a trigger is activated - for instance a commit to a branch.

Tasks you may want to automate are:

- Running tests on the code every time a new commit is pushed to GitHub to make sure everything is working;
- Indexing how much of the code in your repository is covered by tests you wrote, and display a badge showing the percentage in the README;
- Automatically update code running on servers whenever you pushed a commit to a certain branch
- Checking if there have been reports of new security problems with libraries you use every morning;


The script files that contain the commands, or 'workflows' are defined in a YAML file, a common configuration file format, and stored in the .github/workflow directory in your project repository. This is an example of a workflow that is manually triggered and runs one job:

```
name: Print Hello

on: workflow_dispatch
jobs:
  print-hello:
    runs-on: ubuntu-20.04
    steps:
    - name: Print
      run: echo "Hello, world!"

```

This workflow can be run manually by going to the repository's Actions, search for the specific workflow and then clicking the workflow button on it's right. 

With GitHub Actions one could move most of the CI/CD pipelines into Github workflows, or the Continuous Integration/Continous Deployment Pipeline.

**Continuous Integration**
Continuous Integration is the practise of automatically running and testing the codebase when any changes are submitted to an application. An application that has its code stored in a Git repository in Github will have it's code updated by developers pushing changes every day, multiple times a day. For every push to the repository, a set of scripts can be created to test the application's code automatically and integrate it into the project. These scripts help decrease the chances that you introduce errors in your application.

**Continuous Deployment**
Continuous Deployment is a step beyond Continuous Integration. Not only is your application built and tested each time a code change is pushed to the codebase, the application is also deployed continuously and automatically to the production environment. 

## Running tests with Github actions  (CI)

All workflow files will need to be created in the .github/workflows/ folder relative to the root of your repository. To run these tests, the following needs to be done first:

1. The project's repository needs to be brought into the workflow environment. Github supplies the Action 'Checkout repository'. After running this step the repository can be found in the $GITHUB_WORKSPACE.
2. Python needs to be installed on the host machine, which can be done by the 'Setup Python' action that GitHub provides. 
3. Pytest and any other Python modules need to be installed on the host machine. Therefore, specify these modules in the separate requirements.txt file. These can then be installed with in the workflow environment with the ```pip install -r requirements.txt``` in the workflow file  

Based on the above steps, the YAML workflow file (run-tests.yml) should contain the following text:

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

Create this yaml file in the .github/workflow folder and push all your changes to Github, the test should immeditaly run as the trigger is set on 'push'.

If passed, a status badge button can be created to showin within the README file. To do this, check de workflow file and find the 'Create a Status Badge' button in the hamburger on the right. Copy the markdown and place it inside a README.md file at the top level of your repository. Then commit and push your changes. 

 When visiting the repository's page, the status badge shows whether your workflow is currenlty failing or passing.


## Automatically deploy code with Github Actions (CD)

Continuous deployment is a strategy for software releases wherein any code commit that passes the automated testing phase is automatically released into the production environment, making changes that are visible to the software's users.

There are two ways to update the application's code on the server:

1. The code changes will be pushed to Github and if the tests pass, GitHub Actions will automatically PUSH the code to the server using *secure copy*. In order to push, Github actions will need to log into the VPS and execute commands such that the project's code is fully replaced with the latest version. Github can access the VPS through SSH keys: a private key will be added to the repository’s secrets on Github, and a public key can be added to the authorized keys file within the .ssh folder of the user of the server.

    *PRO: The VPS will not need access to the Github repository as in step 2, so you only need to have one public key stored on the server. The key pair can be created on another machine (see more on this below)
    CON: There is no version control on the server, and with secure copy, the updated code will fully replace the code that ran on the server previously*

2. You can set the workflow to execute a script which triggers a pull request from the server itself.  When commits are pushed to Github and the code passes the tests, Github Actions will then need to log onto the server and PULL the updated code into the repository on the VPS. Git needs to be installed on the server

    *PRO: The application's code will be part of a repository that is updated, instead of the whole projects code being replaced by secure copy. Which for some situations, makes it easier to revert changes.  
   CON: In addition to giving Github Actions access to the VPS to execute the pull request, as in option 1, the VPS also needs to be given access to the Github repository. To achieve this you will also need a private key on the server to give access to Github (with the public key added to Github). If the VPS get hacked, hackers will have access to your Github repository with that private key. In general you achieve higher security when less public and private keys are needed to achieve an action*
 


## Creating a git user

 As the root user is the administrative user in a Linux environment, it has very broad privileges. Because of the heightened privileges of the root account, it is discouraged from using it on a regular basis. It is advised to create a seperate user for Github actions. Github Actions can then login as this git user, so that it doesn't have root acccess to the server. 
 
 To create a new git user on the server use the command:
 ```
 adduser git
 ```

The will create a new git user and a new git group. It will ask to create a password for the git user, type a password and press enter. Again retype password then press enter. For other values press enter, if you don’t want to change them.

Go to the home directory and switch to the git user:

```
cd /home/git/
su git
```

## Setting up permissions on /var/www:

The new git user will also need read/write access to the project folder in /var/www so that Github Actions can update the websites code with a scp.


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



## Configure SSH Key-Based Authentication

To set up ssh access for the new user, we need let the SSH daemon know which SSH public keys to accept. This is done using the authorized keys file, and it resides in folder "home/git/.ssh". To create the .ssh folder and authorized_keys file in your git users acount , input:

```
mkdir ~/.ssh && touch ~/.ssh/authorized_keys
```

*Using '&&' chains the commands*. 
*Using the `~` or 'tilde' at the beginning of the path will tell the system to use your home directory, so '~' becomes /home/git/*

The follwoing notes will explain hot to provide Github Action access to VPS over SSH. To log in yourself as this user over ssh - read the notes in Deploymnent.md

**Generating SSH keys**  

First, generate a new SSH key pair. This can be done either on your local computer or on your server - it doesn’t matter since you delete the key afterwards.

To generate the SSH key on the VPS, log into the server and navigate to the .ssh folder. Generate the SSH key here with the following commands:

```
ssh-keygen -t rsa -b 4096 -C "your@email.com"
```
- ssh-keygen: generate a new ssh key
- t is for specifying the type of key to create
- b is for specifying the length of key and
- C is to Request changing the comment. Note: In "your@email.com" use your GitHub email. 


The default file name of the key is id_rsa. It is recommend to change this name so you remember where this key is used. In this case switch the file name to github-actions so you know this key is used for Github Actions. You’ll also be asked to provide a passphrase. Leave this empty since we can’t enter passwords when Github Actions run the SSH command for us.

If you use the ls command now, you should see your keys in the .ssh folder.The public key contains a .pub extension while the private key doesn’t.

**Adding the public key to authorized keys**  

Now add the generated public key (github-actions.pub) into authroized_keys file so machines using the private key (github-actions) can access the server:

```
cat ~/.ssh/github-actions.pub >> ~/.ssh/authorized_keys
```

Append to ~/.ssh/authorized_keys with >>.
Note: . Be careful!
- Grab the contents of github-actions.pub with cat.
- '>>' is to append the content of ssh-name.pub in authorized_keys file

    *Make sure you use double-right-angled brackets (>>) to append and not single-angled brackets (>), as this means overwrite*

**Creating Github Secrets**

After this it is time configure the GitHub Actions Secrets. 

To get the value of your private ssh key type the following inside in your VPS:

```
cat ~/.ssh/ssh-name
```

After copying the ssh private key follow these steps to create the Secrets:

1. Access Your Project Repository
2. Go to Settings, then to Secrets
3. Click “New repository secret” and you’ll be prompted to enter a secret. This secret contains two things — a secret name and the contents. The secret name is used to get the contents later in a Github Actions workflow. It is costum to use uppercase letters with underscores
4. Create 4 New Secrets
  - SSH_HOST: the value is the IP of your server 
- SSH_PORT: the value is the ssh port (by default is: 22)  
- SSH_PRIVATE_KEY: the value is the private generated key  
- SSH_USERNAME: the value in this case is your VPS username (e.g. git)  

Now this private SSH key and the other secrets can be added to a Github Actions Workflow

[READ MORE](https://zellwk.com/blog/github-actions-deploy/)

**Once ssh connection between Github Actions and the VPS is secured, don't forget to delete the private and public key from the origanl place where they was generated. The public key will still be contained withing the authorized_keys file, and the private key should only be stored within Github Actions secrets.**


## Configure GitHub actions to auto-deploy your repository

To Configure GitHub actions to auto-deploy your private or public repository you must create deploy.yml file within the . GitHub/workflows folders if it does not yet exist:
```
mkdir -p .github/workflows
touch .github/workflows/deploy.yml
```

Add the following contents to deploy.yml file:

```
name: Deploy

on: [push]

jobs:
  build:

    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v2

    - name: Copy repository contents via scp  #from github to VPS
      uses: appleboy/scp-action@master  #uses a predefined action package
      env:
        HOST: ${{ secrets.SSH_HOST }}
        USERNAME: ${{ secrets.SSH_USERNAME }}
        PORT: ${{ secrets.SSH_PORT }}
        KEY: ${{ secrets.SSH_PRIVATE_KEY }}
      with:
        source: "."   #current working directory
        target: "/var/www/myproject"
```

[READ MORE HERE](https://dev.to/knowbee/how-to-setup-continuous-deployment-of-a-website-on-a-vps-using-github-actions-54im)

Commit the deploy.yml changes, and push to your repository.
It should build and push to the VPS without any errors.

If you head over to your GitHub repository and click on actions menu you should see a completed job. Now everytime you make changes and push to GitHub that action will run and auto deploy your website.


## Reloading Gunicorn after code changes

Gunicorn does not reload automatically after code changes, so after scp the repository from Github to the server, the systemd service file for the app needs to be restarted manually. 

You can also use a Github Action for executing remote ssh commands. With the ssh-action from Appleboy you can command the Gunicorn service to reload. 

Add the following to the Deploy.yml file:
```

    - name: Executing remote command
      uses: appleboy/ssh-action@v0.1.4
      with:
      host: ${{ secrets.SSH_HOST }}
      USERNAME: ${{ secrets.SSH_USERNAME }}
      PORT: ${{ secrets.SSH_PORT }}
      KEY: ${{ secrets.SSH_PRIVATE_KEY }}
      script: |   #yml syntax meaning multipe lines follow
      ls -lha /var/www/myproject 
      sudo systemctl reload myproject.service

```

The above will first list the projects files and folders which were copied previously with the scp-action. Then it will command the server to reload the systemd service file so that the code changes are refelected on the server. 


**allowing the git user to restart a Gunicorn service file**
As only root users are allowed to restart a systemd service file, the git user needs sudo permission assigned. The sudoers file determines which users can run administrative tasks, those requiring superuser privileges. Using visudo can provide a limited command set to a group for managing a specific service. 

[READ MORE ON VISUDO](https://www.unixtutorial.org/how-to-use-visudo/)

The following line will need to be added to the visudo file:

```
%<group> ALL=(ALL) <Command>
```
This would allow all users in a specific group, on all hosts, to run a specific command as root.

Adding the NOPASSWD argument will enable the git user to operate the service without entering a password first (as this user is only authenticated by an SSH key)

This results in that the following needs to be added the visudo file 
```
%webmasters ALL=(ALL) NOPASSW: /bin/systemctl reload myproject.service
```

[READ MORE ABOUT THIS PROBLEM](https://serverfault.com/questions/772778/allowing-a-non-root-user-to-restart-a-service)



## Trigger one action only after another is completed

The build or 'deploy; job needs to be dependent on the the testing job, with the 'build' job only starting if the 'run_tests' job has sucesfully completed. 

There are 2 options of doing this:

1. Use a second job inside the same workflow.yml together with the 'needs' keyword
2. Create a separate notify.yml workflow that uses the workflow_run event as a trigger.

    [Read more on  stackoverflow](https://stackoverflow.com/questions/62750603/github-actions-trigger-another-action-after-one-action-is-completed)

In general, with big projects, the deployment steps are being kept in seperate workflow files than the testing steps (number 2). Keeping workflows small and simple and referencing other workflows as opposed to combining jobs in one workflow (adding complexity), makes it easier to reuse workflows. For example, on the main production branch, deployment is run after testing. On pull request branches (development branch) however, the testing.yml is often used on its own, and code will be manually reviewed as well before being push to proudction.


To use the workflow_run event as a trigger, you will end up having two separate GitHub Actions workflow yaml files. 

The following needs to be added to the second Deploy.yml file:

```
on: # Only trigger, when the build workflow succeeded
  workflow_run:
    workflows: ["Run Tests"]
    types:
      - completed
```

The name: 'Run Tests' definition of the first yaml file must exactly match the workflow_run: workflows: ["Run Tests"] definition in the second yaml file. This approach will also need to be done on the default branch.

Now, the workflow run is triggered regardless of the conclusion of the previous workflow. If you want to run a job or step based on the result of the triggering workflow, you need to use a conditional with the github.event.workflow_run.conclusion property. To not only run the job when the workflow named "Run Tests" completes, but actually succeeded, add the on 'success' as condition:

```
jobs:
  
  build:

    runs-on: ubuntu-latest
    if: ${{ github.event.workflow_run.conclusion == 'success' }}
```



## Errors encountering when setting up autodeploy

**tar error with scp to server**


- Cannot utime: Operation not permitted
- Cannot change mode to rwxrwxr-x: Operation not permitted

Appleboy@scp-action uses tar (tape archive).  This command is used to rip a collection of files and directories into a highly compressed archive file commonly called in Linux. When restoring files from th archive, tar also attempts to restore timestamp and permissions. 

Originally, the project folders were  manually added as a root user to the var/www/ folder and thus owned by the root user. When Github Action logins to the server as the git user, tar throws an error because it cannot change ownership and permissions for the folders owned root. Removing the original project folder and only then scp the folder over SSH resolves this issue, as the project folder will be created with git as the user and webmasters as the group. 











