---
title: Google Dorking Guide and weekly review
date: 2025-01-08 00:00:00 +0000
image:
  path: https://i.pinimg.com/originals/1f/41/9e/1f419e11c7ae3241f915625b4e8f03a1.gif
categories: [Recon]
tags: [Dorks]
description:  Google Dorking Guide for improving attack surface.
toc: true 
author: l00pz 
---


## Introduction 

Hello guys! Long time hehe see no:) Yeah lack of consistency yea but will be posting new blogs every week.
And btw Happy New Year everyone. I hope 2024 was great for you. I learned a lot of things in 2024 let's see how 2025 will be so I was like let's start some weekly blog series. Yea like frey :P

Last week was a little bit off because of trips and this week I was studying some writups as usual and was hunting yea found some PII leak bug which later was confirmed by frey yes it's PII. Reported it let’s see how it goes will inform you guys. Ok, now enough talk let’s start with the topic? shall we


## Google Dorking 

We all search something on search engines (google, duck duck go, bing, brave, etc), Search engines provide us results on our input feed to it. while searching we only search with words direclty but there are conditions or operators for all search engines for finding exact what we are finding. Yes like finding pdf only in search results `ext:pdf` or `filetype:pdf`.

 Hackers use it for finding sensitive files or even mistakely exposed files. Its also called as **google hacking** or **Google Dorking**. There is whole site on google dorking and people creativly submit their own dorks on this network <a  target="_blank" href="https://www.exploit-db.com/google-hacking-database" >Google hacking website </a>. Hey not always google dorks are used to find senstive file we can also search for those login pages or register pages which are not easily visible on the site.
 
  basic operators and condition every bug bounty hunters should know are :

## Basic Operators

| **Operator**   | **Example**                                                                 |
|----------------|-----------------------------------------------------------------------------|
| `site:`        | `site:example.com` - Searches only within `example.com`.                   |
| `inurl:`       | `inurl:login` - Finds URLs containing the word `login`.                    |
| `intitle:`     | `intitle:"admin panel"` - Searches for pages with "admin panel" in the title. |
| `filetype:`    | `filetype:pdf` - Finds all PDF files.                                       |
| `ext:`         | `ext:doc` - Finds documents with the `.doc` extension.                     |
| `after:`       | `after:2024` - Finds results updated after 2022.                           |
| `before:`      | `before:2023` - Finds results updated before 2021.                         |
| `OR`           | `admin OR login` - Finds results containing either `admin` or `login`.     |
| `AND`          | `admin AND password` - Finds results containing both `admin` and `password`. |
| `*`            | `*admin*` - Matches any word or phrase around `admin`.                     |
| `"`            | `"confidential report"` - Finds the exact phrase `confidential report`.    |
| `-`            | `-site:example.com` - Excludes results from `example.com`.                 |
| `cache:`       | `cache:example.com` - Shows the cached version of `example.com`.           |
| `allinurl:`    | `allinurl:admin login` - Finds URLs containing both `admin` and `login`.    |
| `allintitle:`  | `allintitle:secure login` - Finds pages with both `secure` and `login` in the title. |

---

This were some basic operators which with combination can help us a lot in google dorking and finding information specificly information disclourse or PII.



## Sensitive Dorks 

Sensitive Dorks are used to finding the sensitive files and hidden files that the website does not want to show or mistakenly expose to Google? Yes, such files get disclosed because developers don’t add a deindex tag or if they have a specific endpoint for uploading such documents like admin/information/userdata1.pdf while such endpoint should be disallowed using robots.txt but sometimes developers forget to add such endpoint directory to robots.txt which causes to index on search engines like google.

The dorks I use for finding sensitive files are:

| **Dork**                              | **Purpose**                                                   |
|---------------------------------------|---------------------------------------------------------------|
| `filetype:env "DB_PASSWORD"`          | Find `.env` files with database credentials.                  |
| `filetype:yaml OR filetype:yml "api_key"` | Search for YAML files with API keys.                        |
| `filetype:json "secret"`              | Locate JSON files containing sensitive keys.                  |
| `filetype:log "error"`                | Exposed log files with errors and stack traces.               |
| `filetype:txt "credentials"`          | Text files with stored credentials.                          |
| `filetype:bak OR filetype:backup "password"` | Backup files with potential sensitive data.               |
| `filetype:sql "INSERT INTO"`          | SQL dumps with data inserts (often sensitive).               |
| `filetype:pdf "confidential"`         | PDFs labeled as confidential.                                |
| `filetype:xlsx OR filetype:csv "email"` | Spreadsheets with user data like emails or contact info.    |
| `filetype:conf "password"`            | Configuration files with exposed credentials.                |
| `inurl:/config.json`                  | Direct access to configuration files.                        |
| `inurl:".git/config"`                 | Exposed Git configuration files.                             |
| `inurl:/swagger.json`                 | Open API specs, often containing endpoints and tokens.       |
| `inurl:/wp-content/uploads/`          | Look for exposed uploads in WordPress sites.                 |
| `inurl:"/api/" filetype:json`         | API endpoints with JSON responses.                           |
| `inurl:"/logs/" filetype:log`         | Server logs in public directories.                           |
| `intitle:"Index of /" filetype:sql`   | Directory listings with SQL files. *(Rare but still useful).* |

---

Some my favourties Dorks below : This are so unique and creative ;)

`scanned by camscanner filetype:pdf` - I was thinking about pdfs later while scanning my own document found this watermark on the pdf "scanned by camscanner" yes lot of users use this app for scanning the pdfs and even when they upload on the site with unique name or something `eufebubfbdskksdbk` lmo still we can search it with that watermark. so using `OR` operator I crafted dork <br>
- `"scanned with camscanner" OR "scanned by Adobe Scan" OR "scanned with scanner pro" filetype:pdf OR filetype:jpg`<br>
- `"scan_2023" OR "scanned_doc" OR "scanned_id" OR "docscan" OR "ID_scan" filetype:pdf OR filetype:png`<br>
- `"scanned passport" OR "scanned license" OR "scanned certificate" OR "scanned ID" filetype:pdf OR filetype:jpg`<br>
- `"Epson Scan" OR "Canon Scanner" OR "HP ScanJet" filetype:pdf OR filetype:tiff`<br>
- `"document_2023" OR "scan_2023-*" OR "export_2023" OR "saved_2023" filetype:pdf OR filetype:doc`<br>
- `"passport" OR "tax_form" OR "social_security" OR "license" OR "w2" OR "1099" filetype:pdf OR filetype:xls`<br>
- `"confidential" OR "restricted" AND "scan" OR "passport" filetype:pdf OR filetype:doc`<br>
- `"passport" OR "license" OR "ID" inurl:"/uploads/" filetype:* OR filetype:bmp OR filetype:tiff`<br>
- `inurl:"/documents/" OR inurl:"/scans/" "passport" OR "scan" OR "confidential" filetype:pdf`<br>


## Action-Based Dorks for Web Interactions

Upload-based dorks are dorks for web interactions and where we do interaction specifically we can play here with `file upload` and `https methods`. Or find action-based information and add items or sometimes companies ask for the feedback forms we can use for for testing vulnerabilities. You may use this action-based dork for finding hidden login forms or register forms sometimes companies have partner logins or dev logins you can try auth testing there.


| **Dork**                                          | **Purpose**                                                   |
|---------------------------------------------------|---------------------------------------------------------------|
| `inurl:"/upload"`                                 | Find pages that allow file uploads.                           |
| `inurl:"/file-upload"`                            | Target file upload functionality pages.                       |
| `intitle:"Upload File"`                           | Locate pages that allow users to upload files.                |
| `inurl:"/submit"`                                 | Locate form submission pages.                                |
| `inurl:"/register"`                               | Find user registration forms.                                |
| `inurl:"/login"`                                  | Locate login forms for user authentication.                  |
| `inurl:"/login" inurl:"submit"`                   | Find login forms with "submit" action.                        |
| `intitle:"Post Form"`                             | Look for post submission forms (often used in contact forms). |
| `inurl:"/upload" filetype:jpg`                    | Find image upload pages, potentially vulnerable to file type restrictions. |
| `inurl:"/add" inurl:"submit"`                     | Look for forms used for adding content (such as comments, posts, etc.). |
| `inurl:"/contact" inurl:"submit"`                 | Find contact forms used for submitting inquiries or feedback. |
| `inurl:"/add-item"`                               | Find pages for adding items to databases (e.g., e-commerce).   |
| `intitle:"index of" "upload"`                     | Directory listings that contain file upload features.        |
| `inurl:"/file" "upload"`                          | Look for any page that may deal with file uploads.            |
| `inurl:"/upload" "submit"`                        | Pages that allow users to submit files for upload.            |
| `inurl:"/create" "submit"`                        | Pages that allow content creation, such as posts or items.    |
| `inurl:"/feedback" inurl:"submit"`                | Locate feedback or survey submission forms.                   |
| `inurl:"/api/upload" inurl:"submit"`              | Find pages or endpoints where files are uploaded via forms.  |

## Juicy Files 

Juciy files sometimes might leak internal leaks documents, reports and juicy slack invites, matrix invites links are leaked while creating the manuals for new employyes or for presting pptx. such file may leak important information about the companies so use this dorks for improving your Infromation discolures.

| **Dork**                                          | **Purpose**                                                   |
|---------------------------------------------------|---------------------------------------------------------------|
| `filetype:pdf inurl:"internal use only"`           | Search for PDFs marked as "internal use only" that might be exposed. |
| `filetype:pdf inurl:"not for public"`             | Find PDFs labeled "not for public" but potentially exposed.   |
| `filetype:docx inurl:"internal confidential"`      | Find Word files marked as "internal" or "confidential" but publicly accessible. |
| `filetype:xls inurl:"internal report"`             | Look for Excel files containing internal reports.             |
| `filetype:doc inurl:"private"`                     | Search for Word documents intended for private use but exposed. |
| `filetype:csv inurl:"internal data"`               | Find CSV files containing sensitive internal data.            |
| `filetype:log inurl:"private"`                     | Look for private log files that may be exposed.               |
| `filetype:bak inurl:"backup"`                      | Search for backup files that are intended to be private but exposed. |
| `filetype:md inurl:"internal notes"`               | Find Markdown files containing internal notes or secrets.     |
| `filetype:json inurl:"internal use only"`          | Look for JSON files marked for internal use that are publicly accessible. |
| `filetype:xls inurl:"internal access"`             | Find Excel files that should be restricted to internal use only. |
| `filetype:pptx inurl:"internal presentation"`      | Search for PowerPoint presentations marked for internal use.  |
| `filetype:pdf inurl:"restricted access"`           | Look for PDFs labeled "restricted access" but exposed.        |
| `filetype:txt inurl:"sensitive information"`       | Find text files containing sensitive information not meant for public access. |
| `filetype:xml inurl:"internal resources"`          | Look for XML files containing internal resources or configurations. |
| `filetype:sql inurl:"dump"`                        | Find SQL dump files containing sensitive or database information. |
| `filetype:git inurl:"/.git"`                       | Search for exposed .git directories that might contain sensitive code or history. |
| `filetype:yaml inurl:"config"`                     | Find YAML files that may contain configuration or secret information. |
| `filetype:json inurl:"secret"`                     | Find JSON files containing secret keys or sensitive data.     |
| `filetype:sql inurl:"db_backup"`                   | Look for exposed database backup files that might contain sensitive information. |
| `filetype:git inurl:"repo"`                        | Search for exposed Git repositories containing sensitive code. |
| `filetype:json inurl:"api_keys"`                   | Find JSON files that might contain API keys.                  |



## PII Dorks (Personally Identifiable Information)
PII Is major business loss so companies always make this bug critical as possible because it litreally affects the user privacy or information leakage of the customer which are asset for the company that's why they keep this information secure but sometimes this can be leaked using google dorks check below google dorks for information:


| **Dork**                                                | **Purpose**                                                   |
|---------------------------------------------------------|---------------------------------------------------------------|
| `intext:"email:" inurl:"example.com"`                   | Find pages or documents exposing email addresses.             |
| `intext:"username:" inurl:"example.com"`                | Search for usernames exposed on pages or documents.           |
| `intext:"name:" inurl:"example.com"`                    | Find pages with exposed names, possibly of employees or users.|
| `intext:"contact:" inurl:"example.com"`                 | Search for contact information exposed in documents or pages. |
| `intext:"phone:" inurl:"example.com"`                   | Look for phone numbers exposed in documents or pages.         |
| `intext:"slack.com" inurl:"example.com" intext:"join"`  | Find Slack invite links in documents or webpages.             |
| `intext:"zoom.us/j" inurl:"example.com" intext:"join"`  | Find Zoom meeting invite links exposed in documents or pages. |
| `intext:"teams.microsoft.com" inurl:"example.com" intext:"join"` | Search for Microsoft Teams meeting invite links.             |
| `intext:"google.com" inurl:"example.com" intext:"meet.google"` | Find Google Meet invite links exposed in documents.          |
| `intext:"slack invite" inurl:"example.com"`             | Search for exposed Slack invite links.                        |
| `intext:"zoom link" inurl:"example.com"`                | Find pages with Zoom meeting links exposed.                   |

## Dorks to Find Company Onboarding or Invite Documents (Zoom, Slack, Teams)

Wanna get into the internal teams meeting platforms like zoom/slack/teams you can search for specific dorks for finding the invite links for finding the team invite links where developers join meets/share erros/ feedback and critical infromation of the current development.


| **Dork**                                                | **Purpose**                                                   |
|---------------------------------------------------------|---------------------------------------------------------------|
| `filetype:pdf inurl:"example.com" intext:"join zoom"`   | Find PDFs containing Zoom meeting invite links, possibly for onboarding or team meetings. |
| `filetype:docx inurl:"example.com" intext:"join zoom"`  | Search for Word documents containing Zoom meeting invite links. |
| `filetype:pdf inurl:"example.com" intext:"join slack"`  | Find PDFs with Slack invite links for company workspaces.     |
| `filetype:docx inurl:"example.com" intext:"join slack"` | Search for Word documents containing Slack workspace invite links. |
| `filetype:pdf inurl:"example.com" intext:"join teams"`  | Find PDFs with Microsoft Teams meeting or workspace invite links. |
| `filetype:docx inurl:"example.com" intext:"join teams"` | Search for Word documents with Microsoft Teams invite links.  |
| `filetype:pdf inurl:"example.com" intext:"meet.google"` | Look for PDFs containing Google Meet invite links.            |
| `filetype:docx inurl:"example.com" intext:"meet.google"` | Find Word documents containing Google Meet invite links.      |
| `filetype:pdf inurl:"onboarding" intext:"join us"`      | Look for onboarding PDFs that might contain meeting or workspace invite links. |
| `filetype:docx inurl:"onboarding" intext:"join us"`     | Find Word documents used for company onboarding that contain invite links (Zoom, Slack, Teams). |
| `filetype:pdf inurl:"example.com" intext:"invitation"`  | Find PDF documents that may contain invitation links to meetings or workspaces. |
| `filetype:docx inurl:"example.com" intext:"invitation"` | Look for Word documents containing invitation links for meetings or communication platforms. |
| `filetype:pdf inurl:"example.com" intext:"welcome"`     | Search for onboarding or welcome PDFs with communication platform links. |
| `filetype:docx inurl:"example.com" intext:"welcome"`    | Find Word documents with welcome messages containing join links for platforms like Zoom or Teams. |
| `filetype:pdf inurl:"example.com" intext:"meeting invite"` | Search for PDF documents with meeting invites, possibly containing Zoom links. |
| `filetype:docx inurl:"example.com" intext:"slack invite"` | Find Word documents containing Slack invite links for meetings or channels. |
| `filetype:pdf inurl:"example.com" intext:"onboard" intext:"slack invite"` | Look for PDFs related to company onboarding with Slack invite links. |
| `filetype:docx inurl:"example.com" intext:"onboard" intext:"microsoft teams"` | Find Word documents used for onboarding with Microsoft Teams invite links. |

> **Did you know?** The leak of **Grand Theft Auto VI (GTA 6)** was due to unauthorized access to Rockstar Games' internal chat and meeting platforms, including **Slack**, where the hacker obtained sensitive development materials.
{: .prompt-info }


## Learning How to craft a dork according to requirement :-

Crafting dorks according to the requirement requires time and practice but it can help you in the long run. Custom dorks can help you to detect, adpat and piot according to the `target.com`
- Analyze your target (what tech it use what file storing technologies?) .g., file types, directories, or exposed endpoints
- Use operators like inurl:, filetype:, or intitle: to refine your queries.
- Combine logical operators (AND, OR, -) for more precision.
- Example :
  - Goal : To find all internal use pdfs after 2022 
  - Dork :`"internal use only" OR "Confidential" site:target.com after:2021` 


## Challenge 

I have shared all my dorks and my knowledge on this topic :+) also you can use different search engines like duck duck go for dorking too (without solving that captcha :L). Also before we end this blog this is QUIZ time.

Yeap todays Challenge is to craft a dork for: 

`*.uber.com` wildcard there is a pdf named **Vision Zero** but that is uploaded before 2020 find the pdf and tell me what dork you used to find the pdf and what was subdomain.

Below is programmable google search engine you can try :) challenge here

<div class="gcse-search"></div>


## Conclusion 

You must add google dorking to your recon do it manually and take notes :) understand the website business and structure. See you in the next week ! Keep rocking guys :) I think so I have add too many `:)` smilies in every blog hahah !










<script async src="https://cse.google.com/cse.js?cx=1678d51eac47d42a6">
</script>