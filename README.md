# How to Link Your Registered Domain Name to your Amazon EC2 project

So you have built a funky web application, and hosted it on Amazons EC2 servers. 
And you have registered a memorable domain name for people to access your 
website with. So.... now what?

If you feel completely lost about how to get your domain name to point to your 
server running on EC2, then you are not alone. This was confusing and painful 
journey when I first tried it. But, hopefully I can make it much less painful 
for you. 


## 0. (Optional) Create an elastic IP address for your ec2 server
By setting up an elastic IP address, you can have an IP address for your 
Amazon EC2 project that will never change. You can even reassign that same IP 
address to a different EC2 instance if you like. 

Note that by setting up an elastic IP address, the IP address of your EC2 
instance will be changed. So if you had security settings that restricted the 
IP address from which you could serve your application from, then you will need 
to update those settings after you create an elastic IP. 

Ok, go to your EC2 Dash board, and in the network and Security section, click on 
**Elastic IPs**. 

Click on the **Create a new Address** button at the top. This will create a new 
elastic IP for you. 

Copy and paste the IP address into a test editor, as we will be using the IP 
address later. 

Now click on the **Actions** button, and in the dropdown menu that appears, 
click on **Associate Address** and you will get a popup that looks like this: 

![Image of panel to associate Elastic IP address](LESSON_IMG_DIR/elastic_IP_assosiate_address.png)

- **Instance**: select the EC2 instance you want to use. Then click **Associate**




## 1. Get the Public IP of your EC2 Instance
If you followed the last step to create an elastic IP and have already copied 
the IP address, then you can skip this section. 

In your Amazon AWS dashboard, go into **Instances**, and select the EC2 instance 
you want to have your domain name point to. You will see a panel at the bottom. 
In the **Description** tab, search for the field that says **Public IP**. Copy 
and paste this IP address into a test editor as we will need it later. 




## 2. Prepare Amazon AWS to route traffic from your domain

Go to the Amazon Route 53 dashboard by either: 

- Clicking on **services** then selecting **Route 53**. 
- or using thus url [https://console.aws.amazon.com/route53/]( https://console.aws.amazon.com/route53/)   

From the Route 53 dashboard, go into DNS management. Now click on 
**Create Hosted Zone**. 

A hosted zone will contain information about how the trafic will be routed to 
your sever from yur domain. 

If you see a form that asks you to enter the Domain Name, then enter your 
domain name eg" `example.com`. Otherwise, there should be a button that says 
**Create Hosted Zone**, click on this and the form should appear. 


![Image of create hosted zone panel](LESSON_IMG_DIR/create_hosted_zone.png)


Once you have entered the domain name, click on the **Create** button. 

This will take you to the **Create Record Set** section. This is where we will 
create the settings to route the trafic to our AWS servers.  

This opens up a new form to edit which looks something like this: 

![Image of create record set](LESSON_IMG_DIR/create_record_set.png)


- **Name**: Leave the name blank (unless you want to use a subdomain). 
- **Type**: choose A — IPv4 address.
- **Alias**: Choose no 
- **Value**: Paste the Public IP (or Elastic IP) address you copied into the 
             text editor previously.
- **Routing Policy**: Simple

Now click the **Create** button. 

You should now have three entries in the **Record Set** section. Type **A**, 
**NS**, and **SOA**. Now look at the entry for type NS. It should have several 
addresses for its **value** section. Copy and paste those addresses into a text 
editor, as we will be using it in the next step. 



## 3. Change DNS settings in your Domain Name Provider
Log into the website where you registered your domain name. Ther should be a 
section to change your **DNS** settings. Look for a section that contains the 
**nameservers**. It should have two or more fields that look something like this: 

```sh
ns1.example.com
ns2.example.com
```

The numbers and domain name will differ, but it will usually always begin with 
**ns**. We will be changing these, so click whatever is needed to edit the 
contents of these fields. Delete whatever values were already in there. Now we 
will replace each of them with the addressed we got from Amazon S5. Please note 
the following things: 

- If your domain name register has less entries than the number of addresses 
  you got from Amazon, then just select any of the addresses you like, there 
  is no need to use all of them. 
- If the addresses we copied from Amazon S5 contained a full-stop at the very 
  end, then delete that full-stop when entering it into these fields (but do not 
  delete them from Amazon S5).

Once you have edited and saved these changes, it is just a matter of waiting. 
It might take several hours (or even a few days) for these changes to take 
effect. 

You monitor if the changes have occured using the command line tool `dig` 
(which should be installed by default on an ubuntu machine). Type: 

```
dig example.com
```

And if you see the new NS addresses that you entered, then it is ready. 
Otherwise keep waiting. 

When it is ready, you can access your EC2 server using you new domain name :) 

Hope this tutorial was helpful. 


