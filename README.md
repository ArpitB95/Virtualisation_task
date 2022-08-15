## Virtualisation task

## Create a repo
## create a provision.sh file and within that file write the script which automates the installing all dependencies

## Add instruction in vagrant file to run provision.sh file

## Steps:

### Create a provision.sh file
- Run 'nano provision.sh'
- write script inside this file, which automates the tasks
  '''
  ## Automate download of dependencies

## update & upgrade
sudo apt-get update -y
sudo apt-get upgrade -y

## Install nginx
sudo apt-get install nginx -y

sudo systemctl start nginx  # to start nginx

## Download nodejs version 6 or above
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -  # to navigate the web link to download nodejs version 6 or above

## Install nodejs

sudo apt-get install nodejs -y

## Install pm2

sudo npm install pm2 -g
```

- Now give instruction to vagrantfile to run provision.sh file
```
Vagrant.configure("2") do |config|

 config.vm.box = "ubuntu/xenial64"
# creating a virtual machine ubuntu 
 config.vm.network "private_network", ip: "192.168.10.100"

 # once you have added private network, you need reboot VM- vagrant reload
 # If reload does not work - try - vagrant destroy - then vagrant up
 
## now let's sync our app folder from localhost and sync it to VM
 config.vm.synced_folder ".", "/home/vagrant/app"
 
 ## config provision file 
 config.vm.provision :shell, path: "provision.sh"

end
```

- Now, from your local system run'vagrant reload' ## which will reload the vagrantfile and send the instructions given

- In your local system go to the folder, where test cases are written 


- To run test cases run 'rake spec'
- If every test cases passes, then go to your VM
- run from the location where your vagrant file is located 
- 'vagrant up'
- 'vagrant ssh'

- Now, go to the location where ap.js file is located
- 'cd app'
- again 'cd app'
- run 'ls' , you'll find app.js file


- From here run 
- 'npm install'
- 'npm start'

- You'll see the below outcome

