# Problem Set #6
**Task: Getting All Valid Endpoint of [Website](web-production-d087.up.railway.app "Target Website")**

**Tool: Burp Suite, Gobuster, Seclist, Manual Exploration**

##  What are Endpoints (Website):
In the context of cybersecurity, website endpoints refer to individual URLs or web addresses that are accessible from a particular website. Each endpoint represents a specific function or service that the website offers to its users, such as a login page, a shopping cart, a contact form, or api request urls.

## How can Endpoints be used for in Recon and Web Attack:
    * One can know the services running on a particular domain
    * One can know the type of service used for the domain
    * One can discover sensitive pages and less secure pages
    * One can discover api endpoints and methods of request
    * One can brute force some api endpoint to bypass authencation
    * One can also discover some common vulnerablity exploits (CVE)

## Endpoints found with different tools:
---
### Gobuster and Seclists

---
Gobuster was use for directory enumeration using brute force, while the wordlist used for the brute forcing was gotten from Seclist package which is a collection of multiple types of wordlist used during security assessments.

**Commands used to install the packages**

**Gobuster**

`sudo apt-get install gobuster`

**Seclist**

`sudo apt-install seclists`

**Command used to get endpoints with gobuster**

`gobuster dir  -u  https://web-production-d087.up.railway.app/ -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt`

**Note: explaining the above command**

**dir** is used to specify the directory enumeration mode of Gobuster

**-u** is used to specify the base url for eneumeration

**-w** is used to specify the path to the wordlist

The wordlists collections provided by seclists can be located at **/usr/share/seclists** and for our purpose we are using **directory-list-2.3-medium.txt**

### **Discovered Valid Endpoint**

- [✅]  /user
- [✅] /admin
- [✅]  /questions
- [✅]  /certificate
- [✅]  /redoc

---
### Burp Suite
---
Burp suite was used for intercepting and inspecting requests and responses from these endpoints it was discovered that all these endpoints except **redoc** requires authencation and they are mostly for api rest request except for admin which seems to be a login page for possibly the admin backend.
**The status code return from intruder attacker from Burp Suite is 200 for the above endpoints showing they are vaild endpoints** 

Further visit on those endpoint and intercepting thier request and responses in Burp Suite all except redoc return a **status code of 405** with error message of **"Authencation Credential not provided"** 

---
### Manually Exploration
---
Upon visting the base url is was discovered that url is an  api domain using django rest api framawork. Using chrome browser inspection (Source) no valuable files or endpoints were discovered except static files. But upon manually inspecting the redoc endpoint it was discoverd to contain documentation of the api request available on the domain and certain api endpoints and request methods can be observe from the doc which are as follows:

- [ POST ]  /api/login
- [ POST ] /api/refresh
- [ GET ]  /questions/questions_read/
- [ GET ]  /questions/questions_list/
- [ GET ]  /questions/{id}/
- [ GET ]  /questions/questions_solves_list
- [ GET ] /user/user_list
- [ GET ]  /user/user_read
- [ GET ]  /notifications/{school}
- [ DELETE]  /notifications/{school}
- [ POST ]  /answers/{school}/{id}
- [ GET ]  /certificate
- [ PUT ]  /change-password
- [ GET ]  /user/{id}/
- [ GET ]  /user/{id}/


Overall, endpoints play a critical role in reconnaissance and cybersecurity, and attackers could leverage them as a key source of information for identifying and exploit attacks on the service.