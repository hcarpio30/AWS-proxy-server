# AWS proxy server introduction
Proxy servers are a great way of changing your IP address when you find yourself blocked from websites for reasons such as maximum attempts reached to access an account, or maybe you need to access content that isn't available in your region. Regardless of your reason, creating one using Amazon Web Services (AWS) has never been easier. This tutorial should take approximately 10 minutes!

# Launching your proxy instance
The first step is to sign in to your AWS account or you can sign up if you don't have one already. 

Once you’re signed in, type in “ec2” on the search bar and click on the option that says “EC2 Virtual Servers in the Cloud”.
![step 1](https://user-images.githubusercontent.com/72417975/163872783-007f112b-8cb6-4e6b-8729-9cd51a968a53.PNG)

In the EC2 dashboard, note the region of the server located service health dashboard. You can change the current server region by clicking on the current region name in the top right corner. 
Hit “Launch instance”.
![step 2](https://user-images.githubusercontent.com/72417975/163873201-be52270c-1fa3-4cdc-86f3-dab11b343908.PNG)

Set a name for the instance. 
The OS that we are looking for and going with is Debian. Click on the first option which should be “debian-10-amd64-20210208-542 ami-07d02ee1eeb0c996c”. 
![step 3](https://user-images.githubusercontent.com/72417975/163873746-9e662495-eb08-43e2-addf-c40cc0487dd5.PNG)

Then choose the free tier “t2.micro” instance type. 
![step 4](https://user-images.githubusercontent.com/72417975/163874337-042bd05f-b638-4f21-a6ef-4fabfa407b85.PNG)

To securely connect to your proxy server you will need to generate a key pair. Click on “create new key pair”, name your key pair, and leave the setting as ‘RSA’ key pair type and private key file format as ‘.pem’. A ‘.pem’ file will download onto your computer and the directory of the file will matter going further. NOTE: Do NOT lose or delete this file after the instance is launched, the same key pair file cannot be regenerated! 
![step 5](https://user-images.githubusercontent.com/72417975/163874113-0caf8afe-b162-467a-9996-f2785cd7466a.PNG)

For network settings, enable “Allow SSH traffic from” then select “My IP”. For storage (volumes) and advanced detail settings, you can leave them in their respective default settings. 

Click on “Launch instances” once again and wait for a few minutes for your server to initialize. You can find the current status, as well as other options, of your proxy server on the ‘Instances’ link under the resource's dashboard. 

# Configuring Debain server 
Now to SSH to your proxy server, copy the public IPv4 address of the instance, then open your computer’s command line and go into the directory of the key pair file (.pem). 

Once there, type the following commands: 
```
ssh -i "(keypairfile.pem)" admin@(paste public IPv4 address)

sudo su

apt update

apt upgrade

apt install tinyproxy
```
Now that tinyproxy is installed, we need to edit the config file. 
Type in:
```
cd /etc/tinyproxy/

nano tinyproxy.conf
```
In “tinyproxy.conf” file we need to add our own public IP address because as of right now only localhost can access the proxy server. To make this process quicker, we are going to search “Allow 127.0.0.1” by doing ‘control w’. Under that line type in “Allow (your public IP)” then ‘control o’ to save followed by ‘enter’, ‘control x’ to exit out of the text editor. 
The last command for this ssh session will be: 
```
systemctl restart tinyproxy
```
# Adding security and connecting to your proxy server
The last thing to do on the EC2 dashboard is to go to your security tab of the instance, click on the security groups ID link, then “edit inbound rules”, add a new rule with a custom TCP type, a port range of 8888 and the source being ‘My IP’, click “Save rules”. 
![last step PNG](https://user-images.githubusercontent.com/72417975/163875536-8f919d32-f741-48c9-b33b-e3e7de45abe7.png)

Finally, to connect to your proxy as a user open your computer settings and look for the proxy settings. Add the public IP address of the instance with port number 8888. 

**Congratulations you have just created your very own proxy server with AWS!**
