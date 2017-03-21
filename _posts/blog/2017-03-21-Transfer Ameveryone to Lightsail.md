---
layout: post
title: Transer Ameveryone on Amazon EC2 to Lightsail
categories: blog, AWS, Lightsail
excerpt: Transer Ameveryone on Amazon EC2 to Lightsail
tags: [blog, AWS, Lightsail]
comments: true
share: true
image:
feature:
date: Mar 21, 2017
---

# Transer Ameveryone on Amazon EC2 to Lightsail

Due to the slightly expensive cost of AWS EC2 instant, around \$18 per month, I have to transfer my AmEveryone website to the Amazon Lightsail(around \$5). The following is the tutorial to initial a Lightsail Ubuntu instance. [Original Post](http://sas-and-r.blogspot.com/2016/12/rstudio-in-cloud-with-amazon-lightsail.html?utm_source=feedburner&utm_medium=feed&utm_campaign=Feed%3A+SASandR+%28SAS+and+R%29)

About two years ago we published a quick and easy guide to setting up your own RStudio server in the cloud using the Docker service and Digital Ocean. The process is incredibly easy– about the only cumbersome part is retyping a random password. Today the excitement in virtual private servers is that Amazon is getting into the market, with their Lightsail product. They are not undercutting Digital Ocean entirely– in fact, their prices look to be just about identical. But Amazon’s interface may have some advantages for you, so here’s how to get Docker and RStudio running with Amazon Lightsail.

1. Log in to Lightsail

2. Create an Instance; choose the Base OS, and Ubuntu (as of this writing 16.04 LTS)

3. Name it what you like

4. Wait for boot up. Once it’s running, click “connect” under the three dots. This opens a console window where you are already logged in, saving some headache vs. Digital Ocean.

5. Time for console commands. Type: sudo apt-get install docker.io Then Y for yes to add the new material.

6. Type: sudo service docker start

7. Now you can start your docker/rstudio container. See our earlier blog post or this link for resources. Shortcuts:

a. Plain Rstudio: sudo docker run -d -p 8787:8787 rocker/rstudio

b. All of Hadleyverse: sudo docker run -d -p 8787:8787 rocker/hadleyverse

c. Custom password: sudo docker run -d -p 8787:8787 -e USER=ken -e PASSWORD=ken rocker/hadleyverse

d. Enable root: sudo docker run -d -p 8787:8787 -e ROOT=TRUE rocker/rstudio

8. Important! While the container is starting, go back to the Lightsail tab in your browser and click in the three dots in the “Running” instance to Manage. then click on the Networking tab. In the table of two enabled ports, click on the plus “Add Another”. Leave “Custom” and “All” under “Aplication” and “Protocol”, repectively, and change port range to 8787. Save.

9. The public IP is printed on the Networking page there. Cut and paste into your browser with :8787 appended. Your username and password are both rstudio, unless you changed them. To allow additional users onto your cloud server, see this page.