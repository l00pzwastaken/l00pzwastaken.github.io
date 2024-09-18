---
title: Ultimate Guide for Shodan for Reconnaissance  
date: 2024-09-18 00:00:00 +0000
image:
  path: https://i.pinimg.com/originals/b9/0a/cf/b90acfa130e90e678cf51941d196448f.gif
categories: [Recon]
tags: [Shodan]
description: Ultimate Guide for Shodan for Reconnaissance & orgnization hunting
toc: true 
author: l00pz 
---

## Introduction 

Recon is a crucial phase in the hacking journey, Collection of the target organization's assets and information gathering is what we do First. Shodan is an "Internet search engine for Internet-connected devices" that can be used to find internal services, open ports, third-party services used by companies, IoT devices, webcams, etc. It is a gold mine for security researchers and hunters `:)`. Without wasting time let's start with the Shodan and its actual use and deeper perspective.

## Basic Commands 

The first thing after finalizing the target I do is to use `hostname:` and `certificates` for finding related services and subdomains for the organization.

Let's assume <strong>GOOGLE</strong> as an example for this blog.
```
hostname:target.com
```
![Hostname](https://raw.githubusercontent.com/l00pzwastaken/l00pzwastaken.github.io/main/assets/shodan.png) <br>
```
ssl:target.com
```
OR 

```
ssl.cert.subject.cn: "target.com"
```
![Certificate](https://raw.githubusercontent.com/l00pzwastaken/l00pzwastaken.github.io/main/assets/certificate.png)

```
org:target.com
```
![Organization](https://raw.githubusercontent.com/l00pzwastaken/l00pzwastaken.github.io/main/assets/org.png)

So as you can see there are results related to hostname as well as certificate findings things related to the domain and organization.

> Note: Not all websites found using Shodan are related to the organization, use `dig` or `reverse domain search` to validate the findings.
{: .prompt-info }

You can specify response type like 200,403,500 for including in results or excluding from results:

```
org:target.com http.status:200
```
![http_responses](https://raw.githubusercontent.com/l00pzwastaken/l00pzwastaken.github.io/main/assets/response.png)

Here I have only included 200 https responses for finding active pages you can edit them according to your need for multiple response types you can use `,` between them like this :

```
org:target.com http.status:200,403,500
```

## Excluding Results 

For accurate or specified information, we sometimes need to exudation of ports, response type, org, country, etc.

When you search for something on Shodan you can see on the left vertical side there are countries and other information.

![Information](https://raw.githubusercontent.com/l00pzwastaken/l00pzwastaken.github.io/main/assets/info.png)

When you click on more you will be redirected to the `Facet Analysis` page which makes your work easy and simple for filtering anything.
![Facet](https://raw.githubusercontent.com/l00pzwastaken/l00pzwastaken.github.io/main/assets/facet.png)

As you can see using the drop-down menu you can select different search types like title, response type, port, product, os, country, etc.

![ports](https://raw.githubusercontent.com/l00pzwastaken/l00pzwastaken.github.io/main/assets/ports.png)

You can select unusual ports for example you can choose and exclude results according to the port type.
Sometimes, you may have to exclude the results from the search. Use the `-` minus symbol to exclude the result.
We all know Shodan is full of <strong> Honeypots </strong> so I exclude them first if I find them while searching.

![honeypots](https://raw.githubusercontent.com/l00pzwastaken/l00pzwastaken.github.io/main/assets/honeypots.png)

Other things like ports [ 80,443 ], countries and be excluded using `-` while hunting on the target.

## Favicon Searching 

One of the best and underrated things to cybersecurity researchers is favicon hunting. Favicon are used for personal branding of the Website, they are unique for websites and can be similar for third-party services.

While searching on Shodan you can click on the favicon and get related searches for that `favicon hash` Below is an example for Coca-Cola.

![favicon](https://raw.githubusercontent.com/l00pzwastaken/l00pzwastaken.github.io/main/assets/favicon.png)

```
http.favicon.hash:$hash

```
### Hash generation 

The first thing I do while hunting is generate the favicon hash for that domain and search using that hash for related pages for that target.

What I do, is I visit <a target="_blank" href="https://favicon-hash.kmsec.uk/">Favicon Hash finder</a> and then add the `https://target.com` and it will automatically generate favicon hash for that domain.

![faviconhash](https://raw.githubusercontent.com/l00pzwastaken/l00pzwastaken.github.io/main/assets/faviconhash.png)

With this, I get a lot of results and try to find some services to bypass or custom-build portals by the team of the target organization.

### Default favicons hashes

As I told you guys default web services or microservices use similar favicons or default ones like Nginx, Apache, phpmyadmin, MongoDB clusters, etc are used by popular organizations. I use all these default favicon hashes for finding misconfigs and PII leaks of the target companies. Below are some of the default favicon hash tables.

![Defaulthash](https://raw.githubusercontent.com/l00pzwastaken/l00pzwastaken.github.io/main/assets/favicondefualt.png)


When It comes to finding favicons, scrapts repo by <i>sansatart</i> contains almost most of the 3rd party integrated services and their favicons hash.

<a  target="_blank" href="https://github.com/sansatart/scrapts/blob/master/shodan-favicon-hashes.csv"> Repo for favicons hashes for default services </a>

## CVE/0day Hunting

0-day or CVE is when a product for that particular version is affected by vulnerability on a global level for that version. I mostly use Shodan for 0-day product hacking for organizations that are affected by the 0-day vulnerability.

Here are some steps I use :
> Here I am using Jenkins cli  CVE-2024-23897 RCE to explain my process.
{: .prompt-info }
Step1 : Gathering CVE/0-day 
 After knowing that a product is affected I use some Shodan dorks for getting results for searching that component or service.

 ```
 product: Jenkins
 ```
 ```
 server: apache
 ```
Step 2: Use favicon hashes as I told you before I use favicon hashes for finding these services.
```
http.favicon.hash:$hash
```
Step 3: `Vuln` tag for finding it (only for premium users): Using vuln shodan tag you can find the websites which are affected by the CVE or 0-day attack.
```
vuln: CVE-2024-23897
```
![vuln](https://raw.githubusercontent.com/l00pzwastaken/l00pzwastaken.github.io/main/assets/vuln.png)

Step 4: Report if you find something `:)`


## Conclusion:

Shodan is a great tool and when used properly can improve content gathering and recon information for the target company. I must emphasize if possible use the Shodan premium or education version which will make your workload and be gold mine for your researcher journey ;0. 

 Hope You have gained some knowledge through this post. Meet you next time `:)`.
