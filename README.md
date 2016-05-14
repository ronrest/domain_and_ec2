## (Optional) Create an elastic IP address for your ec2 server
By setting up an elastic IP address, you can have an IP address that for your 
Amazon EC2 instance will never change. You can even reassign that same IP 
address to a different EC2 instance if you like. 

Go to your EC2 Dash board, and in the network and Security section, click on 
**Elastic IPs**. 

Click on the **Create a new Address** button at the top. This will create a new 
elastic IP for you. 

Copy and paste the IP address into a test editor, as we will be using the IP 
address later. 

Now click on the **Actions** button, and in the dropdown menu that appears, 
click on **Associate Address** and you will get a pipup that looks like this: 

![Image of panel to associate Elastic IP address](LESSON_IMG_DIR/elastic_IP_assosiate_address.png)

**Instance**: select the EC2 instance you want to use. Then click **Associate**

