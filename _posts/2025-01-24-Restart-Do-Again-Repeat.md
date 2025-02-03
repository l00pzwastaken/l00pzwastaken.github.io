---
title: Restart Do Again Repeat 
date: 2025-02-03 00:00:00 +0000
image:
  path: https://i.pinimg.com/originals/fe/7c/2d/fe7c2d993bfe37e4f9afa4cdffe1d570.gif
categories: [Recon]
tags: [Subdomain]
description:  Subdomains Enumeration techniques and weekly updates
toc: true 
author: l00pz 
---
Hello, guys I know I am late :) My consistency is not that good, but I am trying to improve it soon. While I was off, I was working on improving my skills. I am going to tell this in this blog, but today's topic is subdomains.  
Yeah, I have read a lot about subdomains and how to find them like a pro.  

## In this blog, I am gonna talk about:
- What did I do this week (month)  
- Subdomain - what is it? :)  
- Different Techniques to find subdomains  
- How to always improve in the subdomain game :)  
- Next week - GraphQL  

## What did I do last month?  
While I try to stay away from Twitter, I have never been so active on Twitter because it causes FOMO and controversies bla bla. However, I joined Twitter specifically to get security researcher tips and blog updates.  

Reading both **API Testing Book** and **Black Hat GraphQL** - I suggest you all read it. It is awesome! Next week, I am gonna talk about GraphQL and other things that I learned.  

Guys, the last PII that I asked Frey and submitted was a **N/A** from the company :(. Let's see if I can catch other bugs this month or not.  

### Projects  
Yo, I do a side hustle and sell projects to final-year students for their final-year projects. So this month, I got **3 clients** hehe!  

### Discord  
Hey, you still have not joined the Discord? Join the Discord! I have added automated writeups, disclosed reports, and 0-day updates :)  
[Join here](https://discord.gg/MYA3SfdQ)  

---

## Subdomain - What is it? :)  
A subdomain is part of a prefix added in the front of a domain :) Yeah, it's a part of the same domain, but it is used for creating a new website.  

**Examples:**  
- **Mail:** smtp.example.com  
- **FTP:** ftp.example.com  
- **Sub:** sub.example.com  

So the subdomain is like a new website for hosting services, protocols, backend stuff, CDNs, etc.  

A web developer or system admin uses the server address and AAA records in the Domain Name System (DNS) for adding a new subdomain with the prefix name.  

---

## Different Techniques to Find Subdomains  
1. **Certificate Parsing**  
2. **Bruteforcing but with permutation**  
3. **Google Dorking**  
4. **Services to find manually**  
5. **Subject Alternate Name (SAN)**  
6. **GitHub Subdomain Finding**  
7. **Tools**  

---

### **Certificate Parsing**  
**Certificate Parsing** OR **Certificate Transparency** is a technique used to find SSL/TLS certificates from websites for getting the subdomains of the current domain certificates. It's a passive search though, it finds a hell lot of results easily.  

BTW, it was created to improve the security of SSL/TLS certificates by making them publicly available by Google.  
In CT, any SSL/TLS certificate created by the issued Certificate Authority (CA) is made public in **Certificate Transparency Logs** (CTL). Later, we can fetch these CTLs for finding more subdomains using passive techniques.  

#### **Services to get all subdomains easily:**  
- [crt.sh](https://crt.sh/)  
- [censys.io](https://censys.io/)  
- [Facebook Certificate Transparency](https://developers.facebook.com/tools/ct/)  

### **Automation**  
I have automated **crt.sh** to find subdomains easily for matching domains. You can do it manually, but automate boring and repetitive tasks!  

**Note:** Read the code before you run it. You need to create a `domains.txt` file containing domains like `target.com`. The script will save the output in `subdomains/sub2.txt`.  

```bash
#!/bin/bash

Domain() {
    local requestsearch
    requestsearch="$(curl -s "https://crt.sh?q=%.$1&output=json")"
    if jq empty <<< "$requestsearch" 2>/dev/null; then
        echo "$requestsearch" | jq -r '.[].name_value' | sed 's/\*\.//g' | sort -u
    else
        echo "No valid JSON response from crt.sh for $1" >&2
        return 1
    fi
}

mkdir -p subdomains

echo "Running crt.sh enumeration..."
while IFS= read -r domain || [[ -n "$domain" ]]; do
    echo "Processing domain: $domain"
    if Domain "$domain" >> subdomains/sub2.txt; then
        total_domains=$(wc -l < subdomains/sub2.txt)
        echo "[+] Total Saved: $total_domains Domain(s) only"
    else
        echo "Error processing $domain" >&2
    fi
    echo ""
    sleep 1  # Add a delay to avoid rate limiting
done < domains.txt

echo "crt.sh enumeration completed"
```

---

### **Bruteforcing but with Permutation :)**  
Bruteforcing is like filling the blanks, but when you don't know what to fill, you use brute force to check if it's correct or not.  

Instead of classical brute-forcing, I read about **permutation-based subdomain discovery** and found a tool called **ripgen**.  
Written in Rust, it finds unique names like `staging.target.com` that are not simply found in DNS or other enumeration methods.  

**Repo:** [Ripgen - GitHub](https://github.com/resyncgg/ripgen)  

We can use `dnsx` to filter only active and working subdomains:  

```bash
cat sub.txt | ripgen > ripgen_domains.txt
cat ripgen_domains.txt | dnsx -t 1000 -silent -o ripgen-results.txt
```

You will have many new subdomains even faster!  

---

### **Google Dorking**  
Google Dorking is a technique for filtering search engine results using search operators.  

Example:  
```plaintext
site:*.example.com
```
- `*` - Wildcard to find subdomains  
- `site:` - Tells the search engine to search within the website  

#### **Useful Links:**  
- [Google Dorks Guide](https://l00pzwastaken.github.io/posts/Google-Dorks-Guide-Guide-weekly-review/)  
- [Bing Search Operators](https://help.bing.microsoft.com/#apex/bing/en-us/10001/-1)  
- [DuckDuckGo Search Syntax](https://help.duckduckgo.com/duckduckgo-help-pages/results/syntax/)  

---

### **GitHub Subdomain Finding**  
GitHub repositories might contain sensitive information, including subdomains.  

#### **Search Query:**  
```plaintext
repo:reponame OR org:organization_name "*domain.com"
```
I found an awesome tool: [GitHub-Subdomains](https://github.com/gwen001/github-subdomains)  
It uses GitHub API tokens to extract subdomains from repositories. I modified it according to my needs!  

---

### **Tools for Subdomain Enumeration**  
1. **Subfinder**  
2. **Amass**  
3. **crt.sh**  
4. **GitHub Subfinder**  

**Note:** Always use API keys! If you don't configure tools with APIs from different services, you might miss **30-40% of subdomains**.  

---

## **Conclusion :)**  
Most people think **recon is a waste of time**, but without proper information gathering, testing isn't effective.  

### **Proper testing methodology:**  
1. Information Gathering  
2. Website Mapping  
3. Exploitation  
4. Post-Exploitation  
5. Report  

Hope you guys learned something new! See you next week :)  
Next week, I am gonna talk about **GraphQL**!  

**Keep learning, and most importantly, follow your passion! :)**  

Peace out ✌️  
