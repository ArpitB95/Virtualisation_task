## What is monolithic architecture ?
- Monolithic applications consist of a single codebase that interacts with a single database. The application’s client interface, single database, frontend, backend and business logic are incorporated into one codebase. There is only one test and deployment pipeline to maintain.


![mon](https://user-images.githubusercontent.com/110182832/184827812-339ebf9f-60c8-4a85-a15d-5ebbec3284ea.png)



## Virtualisation task

- Create a repo
- create a provision.sh file and within that file write the script which automates the installing all dependencies

- Add instruction in vagrant file to run provision.sh file

## Steps:

- Create a provision.sh file
- Run 'nano provision.sh'
- write script inside this file, which automates the tasks
  
````  
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
````


- Now give instruction to vagrantfile to run provision.sh file


````
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
````


- Now, from your local system run'vagrant reload' ## which will reload the vagrantfile and send the instructions given

- In your local system go to the folder, where test cases are written 


<img width="428" alt="test env" src="https://user-images.githubusercontent.com/110182832/184685017-aa4a4a5b-3f54-4d4b-aabf-4a124b2d6b72.png">


- To run test cases run 'rake spec'
- If every test cases passes, then go to your VM
- run from the location where your vagrant file is located 
- 'vagrant up'
- 'vagrant ssh'

- Now, go to the location where ap.js file is located
- 'cd app'
- again 'cd app'
- run 'ls' , you'll find app.js file

<img width="560" alt="ap js" src="https://user-images.githubusercontent.com/110182832/184685111-ced383ff-8ed0-4861-a892-97c6006e20dd.png">


- From here run 
- 'npm install'
- 'npm start'

- You'll see the below outcome

<img width="312" alt="working" src="https://user-images.githubusercontent.com/110182832/184685165-88a040cf-f6c5-427a-b45d-902f8d2d8a01.png">


## Linux variable & Env variable in Linux - windows -mac
- How to check existing Env var 'env' or 'printenv'
- How to create a variable in linux  'Name=Arpit'
- How to check Linux variable  'echo $Nameofvariable'  ## here 'echo $Name'
- Environment variable we have a keyword called 'export'   ## Ex: 'export Last_Name=Bhatt'
- Check specific Env Var 'printenv Last_Name'  outcome would be Bhatt

### How to make Env Variable 'PERSISTENT'
- Research how to make env persistent of your 'first_name' , 'last_name' and
- 'DB_HOST=mongodb://192.168.10.150:27017/posts'

### To make env variable persistent follow theses steps:
- sudo nano ~/.bashrc  ## enter .bashrc directory and add your variables and save it
- Example:  'export variable_name=variable_value'  ## In this case it is 'export first_name=Arpit' , 'export last_name=Bhatt' and 
  'export DB_HOST=mongodb://192.168.10.150:27017/posts'
- save and exit .bashrc file
- run 'source .bashrc' to refresh
- Do vagrant ssh
-check if variable is permanently saved using 'echo $variable_name'


### Two Virtual Machines

<img width="551" alt="new final dia" src="https://user-images.githubusercontent.com/110182832/184918495-92a490b8-1cda-4576-a46b-c06e68b42864.png">



## Linux variable & Env variable in Linux - windows -mac
- How to check existing Env var 'env' or 'printenv'
- How to create a variable in linux  'Name=Arpit'
- How to check Linux variable  'echo $Nameofvariable'  ## here 'echo $Name'
- Environment variable we have a keyword called 'export'   ## Ex: 'export Last_Name=Bhatt'
- Check specific Env Var 'printenv Last_Name'  outcome would be Bhatt

### How to make Env Variable 'PERSISTENT'
- Research how to make env persistent of your 'first_name' , 'last_name' and
- 'DB_HOST=mongodb://192.168.10.150:27017/posts'


- To store variable permanently:
- run 'sudo nano ~/.bashrc'
- Inside bashrc file and add 'DB_HOST=mongodb://192.168.10.150:27017/posts'
- save and exit




 ## Setting up Mongo DB
- Enter in VM db by 'vagrant ssh db'
- Update & Upgrade 
- 'sudo apt-get update-y'
- 'sudo apt-get upgrade -y'

- Now, run following commands
- 'sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv D68FA50FEA312927'

- echo "deb https://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list'

- 'sudo apt-get update-y'
- 'sudo apt-get upgrade -y'

-'sudo apt-get install -y mongodb-org=3.2.20 mongodb-org-server=3.2.20 mongodb-org-shell=3.2.20 mongodb-org-mongos=3.2.20 mongodb-org-tools=3.2.20'

- sudo systemctl enable mongod'
- 'sudo systemctl restart mongod'

- Check status by following command
- 'sudo systemctl status mongod'

- <img width="487" alt="enable" src="https://user-images.githubusercontent.com/110182832/185186175-75a8c7df-f5fa-4a6d-a534-ff04027540b7.png">


- Now go into /etc directory by 'cd /etc'
- Now edit the config file by running 'sudo nano mongod.conf'

- <img width="311" alt="nano config" src="https://user-images.githubusercontent.com/110182832/185186919-b9855058-391e-4640-8bc3-ec1d6ab98c2b.png">

- Edit the config file :
-  under network interface change bindIP to 0.0.0.0 ##which will allow all the requests 

- <img width="220" alt="bindip" src="https://user-images.githubusercontent.com/110182832/185187360-d8f7e645-e0cd-41f8-b3bc-0035ea1bbe48.png">

- Now save and exit the config file

### Now enter in to app VM
- 'vagrant ssh app'
- create env variable DB_HOST=mongodb://192.168.10.150:27017/posts
- To create a permanent variable go in to #/.bashrc file and create variable there using 'export DB_HOST=mongodb://192.168.10.150:27017/posts'
- save and exit .bashrc file

- Now check if it's been saved by running 'printenv DB_HOST'
- If you see the variable then go to 'cd app' and 'cd app' and go where your app.js file is located
- now run 'ls' and ypu'll see 'seeds' file , go into that file
- 'cd seeds'

- now run 'node seed.js'
-now go back to the app/app and run 'npm start' 
