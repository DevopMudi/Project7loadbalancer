
# PROJECT 7 LOAD BALANCER SET UP 

# SETTING UP A LOAD BALANCER
 
We are going to be provisioning two EC2 instances runnning Ubuntu 22.04 and install apache web server on them. 

We will open port 8000 to allow traffic from anywhere and finally update the default of the webservers to display their pubic IP address

Next we will provision another EC2 instance running Ubuntu 22.04, this time we will install Nginx and configure it to act as a load balancer distributing traffic accross the webservers.

A typical example of how our process will work should be as image below but this time  we will only configure two servers and one load balancer(Nginx)


![image](https://github.com/DevopMudi/Project7loadbalancer/assets/149855241/c57c7436-6e8d-4ff0-8aee-944bdc939f66)

# STEP 1 PROVISION EC2 INSTANCE

- Open AWS console and create two EC2 instances, we will consider these instances as our server that is: Apacher 1 server and Apache 2 server as seen in image below


![image](https://github.com/DevopMudi/Project7loadbalancer/assets/149855241/bc1e1aa7-1dc5-4ed1-9f81-a637c690782d)

- On the same page scrow down and click on security, under security option you will see launch-wizard 19 in bracket, click on it to adjust the inbound rule, by opening port 8000 to allow traffic from any where as shown in below image display


![image](https://github.com/DevopMudi/Project7loadbalancer/assets/149855241/38de90df-dbaf-4959-aaa1-62975c13a5c8)

Note: Our servers will be running on port 8000 to allow traffic from any where while load balancer will run port 80

![image](https://github.com/DevopMudi/Project7loadbalancer/assets/149855241/6f2fd2df-9bec-4c03-9e08-67fbe605d7a1)

 

![image](https://github.com/DevopMudi/Project7loadbalancer/assets/149855241/28b5013f-39cf-4a79-a028-5a9e184d0e7f)

- Remember to save rule as below

![image](https://github.com/DevopMudi/Project7loadbalancer/assets/149855241/1a39fa13-8f48-4819-a8b6-9891a8eee602)


# STEP 2: INSTALLING APACHE WEBSERVER

After provisioning both of our servers and have open the neccessary port, its time to install apache software on both servers .

To do so we must first connect each of the servers via ssh, then we can run command on the terminals of our webserver .

To connect to our webserver we click on connect on our instance dashboard we will see the various way we can connect to our servers however, we are connecting through ssh as shown in image below


![image](https://github.com/DevopMudi/Project7loadbalancer/assets/149855241/b900b5cd-069a-4491-a19f-e482544d195c)

- Then we go to our terminal connect through ssh as shown below

 ![image](https://github.com/DevopMudi/Project7loadbalancer/assets/149855241/aae4c7f3-b1f9-46f4-a2cb-c4208ba03f3a)

![image](https://github.com/DevopMudi/Project7loadbalancer/assets/149855241/3897364a-45f2-453f-a4f9-5742933fd464)

- The above is for the apache 1 server, now we do same for apache 2 as below:

  ![image](https://github.com/DevopMudi/Project7loadbalancer/assets/149855241/68c32482-7786-4f38-982c-00cd58b383ae)

  ![image](https://github.com/DevopMudi/Project7loadbalancer/assets/149855241/4710ffae-5c0d-4c68-8a50-43884fe89f91)

Now that we have access to our two servers remotetly we will now update and install appache2 software on each server as below

server 1 update image:

![image](https://github.com/DevopMudi/Project7loadbalancer/assets/149855241/3c4f7f22-6a51-4c8c-ad3b-bbb6060419a1)

server 2 update image:

after which we install the apache2 server software with following command: **sudo apt install apache2 -y** on each machine as below:

server 1:

![image](https://github.com/DevopMudi/Project7loadbalancer/assets/149855241/9535de63-f1f0-4cf0-93cc-90f5d655416e)

server 2:

![image](https://github.com/DevopMudi/Project7loadbalancer/assets/149855241/9a8fa8fc-e9cd-4de9-9845-92c74c84ab91)

- Now that we have install apache software on the two machine server, we now verified if apache is running on both of them by running **sudo systemctl status apache2**

  it should display as below:

  server 1 display:

![image](https://github.com/DevopMudi/Project7loadbalancer/assets/149855241/09ef7815-2c55-442c-af02-eca0f184b70f)

  server 2 display:

![image](https://github.com/DevopMudi/Project7loadbalancer/assets/149855241/54f45aea-f3ce-46fd-b3ed-ceee09d27b2f)

Now that we have apache running actively, we will now configure apache to serve a page showing its public IP

# STEP 3 :CONFIGURING APACHE2 TO SERVE A PAGE SHOWING ITS PUBLIC IP ADDRESS

- We will create a new index html file, the file will contain code to display the public IP of the EC2 instance.

- We will then overide apacahe webserver default html file with our new file

  Steps:
  
  - Using the text editor on the command line input this command: **sudo vi /etc/apache2/ports.conf**

  -Add the new listen directive for port  8000, first type i to switch the editor to the insert mode, then add the listen directive then save the file. 

![image](https://github.com/DevopMudi/Project7loadbalancer/assets/149855241/c9ca5013-69ec-4554-9804-21df69ed8808)

- Next open the file sudo vi /etc/apache2/sites-available/000-default.conf and change the port 80 on the virtual host to 8000 as in below image

  ![image](https://github.com/DevopMudi/Project7loadbalancer/assets/149855241/7f13d90c-2e29-4ee2-8846-2ce1c176403f)

 - Then save the file by pressing escape then !wq: then press enter to save

 - Then restart apache2 with this command :**sudo systemctl restart apache2**

 ![image](https://github.com/DevopMudi/Project7loadbalancer/assets/149855241/af1e482a-f6fb-4c68-88ef-c83c79f74deb)



   # Creating our new file:

   - Open a new index html file with this command: **sudo vi index.html**

   - Switch vi editor into insert mode and paste the html file below: ensure your IP address is type correctly in the html file as in the below

         <!DOCTYPE html>

        <html>
        <head>
             
                  <title>My EC2 Instance</title>

        </head>
            
        <body>

                <h1> Welcome to my EC2 instance </h1>
                         
                <p>Public IP: 51.20.67.155</p>

       </body>

      </html>
Note: it should display as image below: after which you save the file and exit command **:wq!**

![image](https://github.com/DevopMudi/Project7loadbalancer/assets/149855241/647070ac-ec15-4514-9b92-47780f882e02)


- Change file owner of this html file with this command:**sudo chown www-data:www-data ./index.html**

- Overide the default html file with our new html file using the following command: **sudo cp -f ./index.html /var/www/html/index.html**

- Replace the default html file with our new html file with this command: **sudo cp -f ./index.html /var/www/html/index.html**    


- Finally restart the webserver to load the new configuration using the this command: **sudo systemctl restart apache2**


  ![image](https://github.com/DevopMudi/Project7loadbalancer/assets/149855241/4aaa6667-05d4-421b-8e37-24cde23eeb10)


  You should find a page on the browser like the below image.

  ![image](https://github.com/DevopMudi/Project7loadbalancer/assets/149855241/1dc32940-64d9-4d0b-ac02-0008b097ee6c)


# CONFIGURING NGINX AS LOAD BALANCER FOR OUR SERVERS

The steps are as below:

- Provision a new EC2 instance running Ubuntu 22.04, Make sure port 80 is open to accept the traffic from anywhere as image below

  ![image](https://github.com/DevopMudi/Project7loadbalancer/assets/149855241/0c84fd24-d6e1-4e64-8e09-bab4e6256396)

  ![image](https://github.com/DevopMudi/Project7loadbalancer/assets/149855241/1bbd4068-e00b-4ba0-ae9b-32b953ac44ab)

  ![image](https://github.com/DevopMudi/Project7loadbalancer/assets/149855241/3ebe451a-79ec-427d-950a-51d7881c8b1e)

- Connect through ssh:

  ![image](https://github.com/DevopMudi/Project7loadbalancer/assets/149855241/19495c36-4eed-4080-a779-b7a4b255dcf5)


- Install nginx into the instance using the following command :sudo apt update -y && sudo apt install nginx -y

- Afterwhich we check if nginx is active by using this command:**sudo systemctl status nginx**


![image](https://github.com/DevopMudi/Project7loadbalancer/assets/149855241/07a22c90-22ce-4107-a5d0-3c35a3200ab0)




- Open nginx file with the following command: **sudo vi /etc/nginx/conf.d/loadbalancer.conf** , then copy and paste the below script , change the public IP address then save with **!:wq!** and press enter,
        
        upstream backend_servers {

            # your are to replace the public IP and Port to that of your webservers

            server 127.0.0.1:8000; # public IP and port for webserser 1

            server 127.0.0.1:8000; # public IP and port for webserver 2

        }

        server {

                listen 80;

                   server_name <your load balancer's public IP addres>; # provide your load balancers public IP address

            location / {

                proxy_pass http://backend_servers;

                proxy_set_header Host $host;

                proxy_set_header X-Real-IP $remote_addr;

                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

            }
  
        }
    



-    Ensure to change the public IP addresses for server 1, server2 and nginx as display in the image below before saving as stated in the previous step


![image](https://github.com/DevopMudi/Project7loadbalancer/assets/149855241/d268463e-832c-442c-8641-7dd4d1b7f119)


- Then test your configuration with the following command: **sudo nginx -t**

![image](https://github.com/DevopMudi/Project7loadbalancer/assets/149855241/4b2ec7f5-1a94-4968-aab1-0e28d5814a88)


![image](https://github.com/DevopMudi/Project7loadbalancer/assets/149855241/b2d14ff7-317f-46e7-bc02-f80119430bcf)

- if there are no errors, restart nginx to load the new configuration with the following command: **sudo systemctl restart nginx**

- Paste the public IP address of the nginx load balancer on any browser of your choice, you will see the same webpages served by the webservers, as display in image below:

 ![image](https://github.com/DevopMudi/Project7loadbalancer/assets/149855241/beffaaba-fdc3-4637-bade-50b656e0384d)

# References :

- [Digital Ocean: How to set up nginx loadbalancing](https://www.digitalocean.com/community/tutorials/how-to-set-up-nginx-load-balancing)

- [TheGeekStuff: How to Setup Nginx as loadbalancer for Apache or Tomcat for HTTP/HTTPS](https://www.thegeekstuff.com/2017/01/nginx-loadbalancer)




  


















































































