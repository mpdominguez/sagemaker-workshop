---
title: "Configure the Video Recommendation App"
chapter: false
weight: 5
---

Django only allows access via pre-defined source IP addresses. Naturally, these could be open to the internet, but they recommend only exposing it the instance private IP address (for internal calls) and to your front-end load balancer. You already have a reference to the private IP address, so you now need to extract the Load Balancer DNS entry. Go back to the EC2 console screen, but this time select Load Balancers on the left-hand menu; select your Application Load Balancer and in the details screen that comes up select the DNS name and store it for later.

![loadbalancerDNS](/images/loadbalancerDNS.png)

