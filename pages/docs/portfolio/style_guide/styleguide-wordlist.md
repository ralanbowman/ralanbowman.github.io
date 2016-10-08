---
title: In-house Style Guide and Word List
sidebar: second_sidebar
toc: false
permalink: styleguide-wordlist.html
---

As the lone technical writer at a previous employer, I was on my own when it came to choosing tools and setting standards. In order to keep things consistent, I created a style guide and word list. Not wanting to reinvent the wheel, I borrowed heavily from the [O’Reilly Stylesheet and Word List](http://chimera.labs.oreilly.com/books/1230000000969/index.html “O’Reilly Stylesheet and Word List”), and because I knew through user research that the vast majority of our customers were using Windows systems, the [Microsoft Manual of Style](https://www.microsoftpressstore.com/store/microsoft-manual-of-style-9780735648715 “MMoS v4”). 

Starting with this foundation, I created an in-house style guide and word list that could be used by anyone in the company to make sure that any content we created: user guides, release notes, support ticket replies, website copy, JIRA issues, or Confluence pages, all had a consistent style and look. 

---

# Style Guide and Word List 

## Style Guide - Typography and Font Conventions 

| Type of element in document | Typography or font used | Example              | Notes                         |
|-----------------------------|-------------------------|----------------------|-------------------------------|
| Filename                    | Italic                  | *httpd.conf*         | Use for standalone file names | 
| Directory path              | Italic                  | */etc/httpd/conf*    | Use bold and italic if the directory path needs to be emphasized | 
| URLs                        | Italic                  | *http://example.com*, ***example.com*** | Use bold and italic if the URL needs to be emphasized | 
| Links                       | Body font               | [Link to an anchor](#HREF)    | Links (not URLs) to heading anchors or doc sections | 
| Linux commands              | Monospace bold italic   | ***`cd /etc/httpd`*** | Make sure that any dashes (`---`) show up correctly | 
| Command names               | Monospace               | `cat`, `cd`, `netstat -rn` | This is for standalone command names. Make sure that any dashes (`-`) show up correctly | 
| Command line (CLI) placeholder names | Monospace italic | `login:` *`username`* | CLI placeholders are where the user will have to type something in at the command line |  
| GUI placeholder names | Italic                        | Login: *username*     | GUI placeholders are where the user will have to type something in from a GUI (dialog box, etc.) |
| E-mail address             | Italic                   | *user@example.com*    | Make the e-mail address a `mailto` link if needed | 
| Service names              | Lowercase italic         | *glassfish*, *jboss*, *sendmail* | These are the names of the application service, not the proper name of the application |
| Page or document titles    | Bold                     | **Name of Page**, **Document Title** | Use for the name of a page or document | 
| GUI navigation             | Bold italic, right angle bracket              | ***Path*** > ***to*** > ***location*** | Use to show navigation through a GUI (similar to breadcrumbs) |


---

## Word List 

**Numbers/Symbols**    
[ ] left/right bracket     
{ } left/right brace     
< > angle brackets     
~ tilde      
32 bit     
64 bit    
  
**A**    
Apache web server    
  
**B**    
back up (v)   
backup (n)   
bandwidth  

**C**    
Confluence     

**E**    
e-mail/email (be consistent)    

**F**    
FTP (n)    
FTP client     
FTP server    
ftpd (service)    

**G**      
GB (gigabyte)        
Git (n)        
git (service)       
GlassFish (n)       
glassfish (service)      

**H**      
home page     
hostname     
HTML    
HTTP (server)     
httpd (service)    

**I**    
I/O     
Internet     
intranet     
IP     
IPsec    
  
**J**  
Jelastic (n)   
JBoss (n)   
jboss (service)   
JIRA  
  
**K**  
k (1000)  
K (1024)  
kb (kilobit)   
KB (kilobyte)  
  
**L**  
Liferay   
Liferay-Portal-GlassFish (n)   
liferay-portal-glassfish (service)   
Liferay-Portal-Tomcat (n)   
liferay-portal-tomcat (service)  
  
**M**  
Mb (megabit)   
MB (megabyte)   
MySQL (n)   
mysql (service)  
  
**P**  
Page (a Confluence Page)   
PaaS  
Platform as a Service   
PostgreSQL (n)   
postgresql (service)  
  
**Q**  
QTS  
  
**R**  
R1Soft  
rails (service)   
RoR (n)  
Ruby (n)  
Ruby on Rails (n)  
  
**S**  
Sendmail (n)  
sendmail (service)  
Space (a Confluence Space)   
Subversion (n)  
SVN (n)  
svn (service)  
  
**T**  
Tomcat (n)   
tomcat (service)  
  
**V**  
Virtual Cloud Server (the hosting platform)  
Virtual Machine (deprecated, use Virtual Server)   
Virtual Machine in the Cloud (the hosting platform)   
Virtual Server  
Virtuozzo  
VM (deprecated, use VS)  
VPS  
VS  

**W**  
web page   
web server  
website   
WildFly 8 (n)  
WildFly 10 (n)  
wildfly (service)  
Windows 2000   
Windows 7   
Windows 8   
Windows 95   
Windows 98   
Windows NT   
Windows Vista   
Windows XP  

**X**  
xinetd   
xinetd.conf   
xinetd.d  

**Y**  
Yum (n)  
yum (service)  


