---
title: Ultimate Guide for Shodan for Reconnaissance  
date: 2024-09-17 00:00:00 +0000
image:
  path: https://i.pinimg.com/originals/b9/0a/cf/b90acfa130e90e678cf51941d196448f.gif
categories: [Recon]
tags: [Shodan]
description: 
toc: true 
author: l00pz 
---

## Introduction 

Recon is crucial phase in hacking journey, Collection of target organization's assets and information gathering is what we do First. Shodan is "Internet search engine for Internet connected devices" which can be used to find internal services, open ports, third-party services used by companies, IOT devices, webcames,etc. It is gold mine for security researchers and hunters `:)`. Without wasting time let's start with the Shodan and it's actual use and deeper perspective.

## Basic Commands 

First thing after finalizing the target I do is using `hostname:` and `ceritificates` for finding related services and sundomains for the the organization.

Let's assume <strong>GOOGLE</strong> as example for this blog.
```
hostname:target.com
```
![Hostname](/_site/assets/shodan.png) <br>
```
ssl:target.com
```
OR 

```
ssl.cert.subject.cn:"target.com"
```
![Certificate](/_site/assets/shodan.png)

```
org:target.com
```
![Organization](/_site/assets/shodan.png)

so as you can see there are result related to hostname as well as certificate findings things related to the domain and organization.

> Note: Not all websites found using shodan are related to the organization, use `dig` or `reverse domain search` for validating the findings.
{: .prompt-info }

You can specify response type like 200,403,500 for including in results or exluding from results:

```
org:target.com http.status:200
```
![http_responses](/_site/assets/shodan.png)

Here I have only included 200 https reponse for finding active pages you can edit it according your need for multiple response type you can use `,` between them like this :

```
org:target.com http.status:200,403,500
```

## Excluding Results 

For accurate information or specified information we sometimes need to exludation of ports, response type, org, country, etc.

When you search something on shodan you can see on left vertical side there are countries and other information.

![Information](/_site/assets/shodan.png)

when you click on more you will be redirected to the `Facet Analysis` page which makes your work easy and simple for filtering anything.
![Facet](/_site/assets/shodan.png)

as you can see using the drop-down menu you can select different search types like title, repsonse type, port, product, os, country,etc.

![ports](/_site/assets/shodan.png)

you can select unusual ports for example you can select and exlude results according to the port type.


Sometimes, you may have to remove exlude the results from the search use `-` minus symbol for removing the things