**Deploy a Static Website using Amazon S3 and Route 53: A Step-by-Step AWS Project**
====================================================================================

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*z8soa-OwIOMJuLPz.png)

[Reference](https://medium.com/@nikhilsiri2003/deploy-a-static-website-using-amazon-s3-and-route-53-a-step-by-step-aws-project-01afdfbe5495)

by [Nikhil Raj A](https://medium.com/@nikhilsiri2003?source=post_page---byline--01afdfbe5495---------------------------------------)

**Introduction**

**Amazon Simple Storage Service (S3)** is a scalable, secure, and highly available cloud storage service provided by AWS. One of its powerful features is the ability to host static websites directly from an S3 bucket, making it an excellent choice for developers looking for a cost-effective and serverless hosting solution.

In this step-by-step AWS project, we will walk through the process of deploying a static website on Amazon S3. This involves creating an S3 bucket, configuring it for website hosting, setting up permissions, and making the site publicly accessible. Additionally, for enhanced performance and security, we can integrate Amazon CloudFront (a Content Delivery Network), enable HTTPS using AWS Certificate Manager (ACM), and configure Route 53 for a custom domain.

By the end of this guide, you will have a fully functional static website running on Amazon S3, demonstrating how AWS can be leveraged for scalable and low-cost web hosting.

![Hosting Static Site using AWS S3 and Route 53](https://miro.medium.com/v2/format:webp/1*nFMgyQK2HVP88MYuCq2YXQ.png)

**Prerequisites**
-----------------

To get started with this project , you need the following:

*   AWS Account .
*   A custom registered Domain.
*   Code for the Webapge or Website.

What is a static-site?
======================

A static site is a website made up of web pages with predetermined content that is always the same for every visitor. In other words, a static site’s content is pre-rendered and presented to users in its current state without any dynamic or in-the-moment processing.

Static sites can include multimedia components like images and videos and are frequently created using HTML, CSS, and JavaScript. The main distinction between a static site and one that changes in response to user input or interaction is that the content on a static site is fixed.

What is a domain name?
======================

A domain name is a distinctive, spelt-out name that identifies a particular website on the internet. Without having to memorize long IP addresses, it is used to quickly find and access websites.

The actual name and the domain extension make up the two main components of a domain name. The name “example” and the domain extension “.com” are both present in the domain name “example.com,” for instance. The domain extension also referred to as the top-level domain (TLD), identifies the website’s category or type.

Simple Storage Service (S3)
===========================

S3 is a cloud-based object storage provided by AWS which offers industry-leading scalability, data visibility, security and performance. It allows you to store and retrieve large amounts of data over the internet. You can reduce costs, organize data, and set up precise access controls to satisfy unique business, organizational, and compliance requirements using cost-effective storage classes and simple management tools.

**Step 1 : Go to Route 53 and Create a Root Custom Domain**
-----------------------------------------------------------

Simply type **Route 53,** and you should see the service

![Route 53](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*wHFwqHPIrI5Xo1CFhnApkQ.jpeg)

1.  Create a custom Domain by providing the desired name that you want to have as Domain name . I have a domain named as (**travel-website.click**).

![Registering the Domain](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*KCwe_lD93SnNw4kj_CBCOg.png)

2. After the selecting the desired domain , click on **Create hosted zone** and your hosted zone will be created.Once the hosted zone is created you can see an interface like this.

![Hosted zone](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*ZuUpdaCwADWzZHAMTJylFw.png)

**Step 2 : Creating a S3 Bucket with custom domain name.**
----------------------------------------------------------

![AWS S3](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*jluB68FJTWvYFJJxm7DdxA.jpeg)

1.  Click on **Create Bucket** to create a bucket , where we can upload the files that are required for hosting a site .
2.  Provide the S3 bucket name which should match both Route 53 domain name and as well as the S3 Bucket name . If the names are different from each other then, the it won’t work as expected.

**Reasons for not working as expected :**
-----------------------------------------

*   **S3 Static Website Hosting Requires Bucket Name to Match the Domain Name
    **When you host a **static website** on Amazon S3 and want to use a **custom domain name** (e.g., `example.com`), your **S3 bucket name must exactly match your domain name** (`example.com` or `[www.example.com](http://www.example.com).)`[).](http://www.example.com).)
*   **Route 53 Won’t Automatically Link to an Arbitrary S3 Bucket
    **Route 53 acts as a **DNS service**, mapping domain names to resources (like S3, EC2, CloudFront).
    If your bucket name does not match your domain, you **cannot directly create an alias record** in Route 53 pointing to the S3 website endpoint.

![Providing name for S3 Bucket](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*dziYfroi5Y-R5RH7t_QtTg.jpeg)

3. Disable Block public access so that anyone with the bucket link can access the content inside the bucket. Check on **I Acknowledge that …**

4 . Then simple click on **Create Bucket** , then the bucket gets created .

**Step 3 : Upload the Files and Enable the Static web hosting.**
----------------------------------------------------------------

1.  Click **Upload** and then simply drag all the folders and files.
2.  After uploading all the files and folders , you can find the files and folders that have been uploaded to the bucket under the bucket itself . It also shows the name of the files , size and also type of the file .

![uploaded files into S3 Bucket](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*T7MeUuTnwTJAX3NKjYXJBw.jpeg)

3 . **Attach the bucket Policy ( To make the bucket public )
**You need to write a bucket policy in order to make the bucket publically accessable , and bucket policy is written in JSON format.

![Bucket Policy (JSON)](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*WQSPJbv-_6TTNK-nJXYqqQ.png)

4. Under **permissions>Bucket policy** the policy can be added to the required S3 bucket, to make it access publically. Click **save** to save the changes.

My Bucket Policy :

![My Bucket policy (JSON)](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*e34o5EMqLOmiwvaV_VTSlA.png)

5. **Enable Static web hosting**

There is one more step which is very much crucial for both hosting and routing with the domain. To enable static web hosting go to **Properties** and scroll down to the end , there you can see a option called Static web hosting . click **Edit**

Enable **Static website hosting** and specify **index.html** inside **Index document**. Then simply click on **Save Changes**.

![static web hosting](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*jwhfee-Ci1SGJhUrKvzgFg.png)

Now you should be able to a **Bucket website endpoint.** When you copy paste the link into browser you would see a beautiful site.

![Bucket website endpoint](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*rQNM1QWM1n3CWVaR8eBZRg.jpeg)

**Step 4 : Create an A Record**

Go back to Route 53 where you have registered domain and created a hosted zone, there you can see a option called **Create Record .**

![creating an A Record](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*PAPQ97RCTs1jm2p5DsKNTw.jpeg)

Under **Route traffic to** choose **Alias to S3 website endpoint**

Under **Choose Region**, choose the region where you created the S3 bucket. In my case, it _ap-south-1_

Now, under **Enter S3 endpoint**, you should see your endpoint with a **domain name (travel-website.click)**. For some reason, if you don’t see th endpoint listed, you can visit this [site](https://docs.aws.amazon.com/general/latest/gr/s3.html#s3_website_region_endpoints) and copy the **Website Endpoint** based on your S3 Bucket deployed Region name.

For **Evaluate target health**, choose **No**.

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*BkeedMZrYSEL1XdWRzURpg.png)

Then Click on **Create Records** , the record will be created with **S3 website endpoint** attached to it .

![A Record created](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*Rf92yQIUza_87rOEF2Jmqg.png)

Now , you can see your (A record) under other records containing different types of records.

**Result :**

Visit your domain name(**travel-website.click**) by typing or pasting the domain name into the browser in order to access the content . Make sure the domain is registered if not the domain will throw an error.

You can also make your Domain secure by requesting a **SSL Certificate** under **certificate manager** and attaching it to the domain. And SSL certificate is required to encrypt data transmitted between a website and a user’s browser, ensuring secure communication and protecting sensitive information like passwords and payment details.

![Simple diagram of how the SSL Certificate works](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*eJQ02EZBXTl76fHWXfqvLw.png)

**What is SSL Certificate ?**

An SSL (**Secure Sockets Layer**) certificate is a digital file that authenticates a website’s identity and enables secure, encrypted communication (HTTPS) between a web server and a browser, protecting sensitive data during transmission.

![Site accessed using Domain](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*S-oyxdKnBodeLu7IxuBMLw.jpeg)

It’s that simple to host your site with a proper domain with the help of **S3** and **Route 53**.

Conclusion
==========

In this post, we hosted our static site using **AWS S3** and configured **Amazon Route 53** to point our domain to the S3 bucket. Thus, the site can be easily visited using our domain name.
