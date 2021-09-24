# Adding users to EC2 instances with SSH Access and connecting to them using Ansible

## Step 1: Create 3 nodes- 1 master or Ansible management control node and 2 server nodes or worker nodes(using ec2 instances in AWS)
- In this exercise we have named them as ansible-host(a Red-hat RHEL host), webserver and dbserver(Amazon 2 Linux AMIs) respectively.
<img src="https://github.com/kuluruvineeth/Devops/blob/main/ansibleServerConnectionExercise/screenshots/2.png">

## Step-2: Connect to the Ansible host using SSH client method or AWS EC2 console method.

```
sudo yum update
```
## Step-3: Installing ansible in remote ansible host
```
sudo yum install ansible
```
## Step-4: Confirm the ansible installation
```
ansible --version
```
## Step-5: Check the present working directory and move back to the root location

## Step-6: Confirm the ansible installation
```
pwd
cd ..
cd ..
ll
```
## Step-7: Go to the etc/ansible folder
- Now go to the hosts file
  ```
  vi hosts
  ```
-  Now, update the host file with:
   ```
   [webservers]
   <Public_IP_Address_of_webserver_node>
   [dbservers]
   <Public_IP_Address_of_dbserver_node>

## Step-8: Connect to the webserver node from aws console
- Go to the superuser
```
sudo su
```
- Now add a user
```
useradd -d /home/ansibleweb -m ansibleweb
```
- Now create a password for this user
```
passwd ansibleweb
```
- Next, change the user to ansible web. Then go to the ansibleweb folder and create a .ssh file, change its user access and generate the rsa private and public keys
```
su - ansibleweb
cd home/ansibleweb
mkdir .ssh
chmod 700 .ssh/
ssh-keygen
```
- Now move to the .ssh folder, then create a file named authorized_keys and copy paste the public key(got from cat id_rsa.pub)
```
cd .ssh
ll
cat id_rsa.pub
vi authorized_keys
```
## Step-9: Now switch back to ansible-host instance. Then do
- Go to the superuser
```
sudo su
```
- Now add a user
```
useradd -d /home/ansiblehost -m ansiblehost
```
- Now create a password for this user
```
passwd ansiblehost
```
- Go to the root location and chnage the owner of ansible folder
```
cd ..
cd ..
cd etc/ansible
chown ansiblehost:ansiblehost .
```
- Create  a remote-web.key file in the ansible folder and copy paste the pivate key of the webserver instance(node)
``` 
vi remote-web.key
cat remote-web.key
```
- In ansible host chnage the permissions of remote-web.key
``` 
chmod 600 rempote-web.key
```
- Go to the webserver. 