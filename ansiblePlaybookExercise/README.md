# Ansible Git Example – Checkout code from Git Repo Securely

## Step1: Configuring Git login
- First do **git init** in some directory from where you wish to login and login using the below method using the username and security token on the url(for more info on login refer the gitExercise folder)
  - https://user:token@github.com/path

## Step2:  Creating Ansible vault to Store the Git username and token
<img src="https://github.com/kuluruvineeth/Devops/blob/main/ansiblePlaybookExercise/screenshots/1.png">
<img src="https://github.com/kuluruvineeth/Devops/blob/main/ansiblePlaybookExercise/screenshots/2.png">

## Step3:  The Ansible Git Example Playbook
```
---
  - name: Install and Launch the Simple NodeJS Application
    hosts: nodeserver
    vars_files:
       - secrets.yml
    vars:
       - destdir: /apps/SampleNodeApp
    tasks:

       - name : install Node and NPM
         become: yes
         register: ymrepo
         yum:
           name: nodejs
           state: latest

       - name : validate the nodejs installation
         debug: msg="Installation of node is Successfull"
         when: ymrepo is changed

       - name: Version of Node and NPM
         shell:
            "npm -v && node -v"
         register: versioninfo

       - name: Version Info
         debug:
           msg: "Version info {{ versioninfo.stdout_lines }}"
         when: versioninfo is changed

       - name: Download the NodeJS code from the GitRepo
         become: yes
         git:
            repo: 'https://{{gituser}}:{{gitpass}}@github.com/AKSarav/SampleNodeApp.git'
            dest: "{{ destdir }}"

       - name: Change the ownership of the directory
         become: yes
         file:
           path: "{{destdir}}"
           owner: "vagrant"
         register: chgrpout

       - name: Install Dependencies with NPM install command
         shell:
            "npm install"
         args:
            chdir: "{{ destdir }}"
         register: npminstlout

       - name: Debug npm install command
         debug: msg='{{npminstlout.stdout_lines}}'


       - name: Start the App
         async: 10
         poll: 0
         shell:
            "(node index.js > nodesrv.log 2>&1 &)"
         args:
           chdir: "{{ destdir }}"
         register: appstart

       - name: Validating the port is open
         tags: nodevalidate
         wait_for:
           host: "localhost"
           port: 5000
           delay: 10
           timeout: 30
           state: started
           msg: "NodeJS server is not running"
<img src="https://github.com/kuluruvineeth/Devops/blob/main/ansiblePlaybookExercise/screenshots/3.png">
```

## Step4:  Launch the Playbook with Ansible Git
```
ansible-playbook gitexample.yml --ask-vault-pass
```
<img src="https://github.com/kuluruvineeth/Devops/blob/main/ansiblePlaybookExercise/screenshots/4.png">
<img src="https://github.com/kuluruvineeth/Devops/blob/main/ansiblePlaybookExercise/screenshots/5.png">

## Step5: Validate the Deployment
<img src="https://github.com/kuluruvineeth/Devops/blob/main/ansiblePlaybookExercise/screenshots/6.png">
