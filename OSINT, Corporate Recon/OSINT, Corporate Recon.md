# Open-Source Intelligence

`Open-Source Intelligence` (`OSINT`) is a process for finding publicly available information on a target company and/or individuals that allows identification of events (i.e., public and private meetings), external and internal dependencies, and connections. OSINT uses public (`Open-Source`) information from freely available sources to obtain the desired results. Some sources for collecting data are, for example, mass media such as television, radio, print media, or the Internet. The information collected from these sources will be analyzed, evaluated, and linked to each other to gain the necessary insights about our target.

The focus of OSINT lies in the word "`Intelligence`," which means constructing relationships between individual pieces of information from which we can create specific patterns and profiles about the target. The art here is to look `behind the scenes` and `think outside the box`.

It is essential to understand that the individuality and design of each company are usually entirely different. Therefore the focus of this Module is to understand and internalize the OSINT process's principles in the best possible way. We do not have to pay attention to the resources or tools we use since those can differ significantly, but rather on `the way` we find the information and use it during our assessments and how we present it to our clients to provide them the most value.

## Definition of OSINT

There are many ways of describing OSINT. Many definitions of it are unfortunately wrong and could lead to `illegal` activities in most countries. In most guides and resources, we only find introductions of individual resources and tools aimed at providing us with all the most necessary information. With this, only the "`Open-Source`" part of `OSINT` is covered, and the "`Intelligence`" behind it is completely ignored or skipped. Knowing which tools are available and which ones we choose that best fit our own approach, with a bit of luck, only covers `10%` of what is needed for `OSINT`. We will see and use some resources and tools throughout this Module, but no tool can replace the core elements. All `OSINT` tools are based on specific information resources and are therefore limited to those. Those tools may help us to find some information quickly, but, as mentioned before, this is only a tiny fraction of the whole picture.

`Open-source information or open sources, is any data that can be obtained from public sources by anyone without any restrictions, whether for free or commercially, in a legal and ethically acceptable way.`

In `OSINT`, information is categorized and linked together to form a logical connection. For example, it could be an employee of a company with a developer role who uses specific source code components from their private Github repository for the company infrastructure. Whether any particular tool will even show us this source is questionable. However, the intelligence we apply manually allows us to logically relate such details to our target company.

It is `essential` to understand `precisely` what `OSINT` is and what it is `not`. OSINT is based `only` on the passive gathering of information about the target company from publicly available and (without registration or authentication required) accessible sources. To make it clear, this is mainly about interaction with the target and not about third parties. Information that is disclosed on different platforms by our target company (for example, on LinkedIn) can be used with a registered account, of course. If, for example, we are dealing with internal forums, for which we need to register to access internal information (confidential & non-public), we are actively interacting with our target, which is no longer part of OSINT. There must be no active enumeration or interaction with the target, such as brute-forcing subdomains or similar, during the OSINT phase. This type of interaction with the target is part of the active enumeration phase. The `OSINT` process should consist of only using publicly available information.

Note: In this Module, we will see some examples of how this information can be used in later phases of our penetration test. Examples that will have to do with phone calls or physical access are, therefore, not part of the OSINT process.

`It cannot be stressed strongly enough how important it is to differentiate between these categories.`

Even if the customer has an open S3 bucket running on AWS as an example, which was not contractually defined in the scope of work and we identify it by brute-forcing the names, we are already a violation of the contract. This type of activity could lead to a problematic situation with significant consequences.

A distinction `must` be made between tools that `actively interact` with the available resources of the target company and use different methods to retrieve information or use the `disclosed information` to connect it to other parts of the databases. For example, when a tool tries to find vhosts or subdomains by brute-forcing, it is an active scanning method that sends requests to the DNS servers to confirm the existence of one of these vhosts or subdomains and is, therefore `no longer part` of OSINT.

However, when a tool extracts the SSL certificate from a web server and compares the data in databases such as [crt.sh](https://crt.sh/), also known as `Certificate Transparency`, we are not performing an active scan against the target company. In this case, we are merely using the viewable information to obtain additional information from third parties.

Below is a table that shows a few examples of when it is `OSINT` and its legality.

|**Activity**|**OSINT?**|**Legality**|
|---|---|---|
|Looking for employees on Social Networks|`Yes`|Legal. Open-Source information.|
|Viewing company's website|`Yes`|Legal. Open-Source information.|
|Directory brute-forcing company's website|`No`|Illegal without permission since it is an active enumeration/scanning process.|
|Brute-forcing company's subdomains|`No`|Illegal without permissions since it is an active enumeration/scanning process.|
|Looking at certificate transparency logs to identify subdomains|`Yes`|Legal. No direct interaction with the target company.|
|Using third-party services that have collected information about the target company|`Yes`|No direct interaction with the target company.|
|Using third-party services that scan the target company|`No`|Illegal without permissions since it is an active enumeration/scanning process requested by us.|
# OSINT Methodology

During our penetration tests, we use OSINT to understand our target company better. The better we understand our target, the easier it will be to adapt our attacks to it, especially when it comes to `Social Engineering`. Suppose we were to go ahead and scan our customer's network. It is common for well-secured networks to have integrated IDS/IPS that would immediately ban us for several hours, if not days, based on our scan. Triggering this type of alarm right at the beginning does not show the professionalism of our activities and even contradicts it.

To perform OSINT efficiently, we need a structure that shows us the essential aspects and dependencies of the information resources and core information. The following diagram shows the critical core elements required and can, of course, contain additional components.
![[Pasted image 20260525121939.png]]
Here we distinguish between the `Core Elements` we need and the `Information Resources` from which we can extract the corresponding core information.

`Core Elements` are pieces of information that give us a better picture of the company and its infrastructure. These can be names and versions of software applications, servers, names, user names, hashes, URLs, passwords, and much more.

`Information Resources` are resources from which we obtain these Core Elements. These information resources can be websites, social networks, documents, scripts, and many others.

|`Core Elements`|`Information Resources`|
|---|---|
|Company Information|Home Page|
|Infrastructure|Files|
|Leaks|Social Networks|
||Search Engines|
||Development Platforms|
||Leak Resources|

This helps us to keep an eye on which resources we can use most of the time and tick them off one by one. However, we have to keep in mind that any information we find will lead to repeated searches and new resources for more detailed information. We can think of it as a growing tree where the core elements represent the branches, and as we add branches, we will have more nodes to connect to perform much more thorough and detailed research.

Therefore, it is highly recommended to organize the whole process in `cycles` and repeat the search with the new information for each new cycle. This systematic approach allows us to have a structured workflow and clean documentation that will enable the client and us to understand precisely how the data and information have been obtained.

For example, we can take the company name from its `website` and then search on `social networks` or use `search engines`. We can also search for it on different `development platforms` or different `forums`. Once we have gathered the core information about it, we start the `cycle` again, this time using the newly gathered information during the search for related information for the corresponding category. We try to get from the `rough to the detailed` to get a general overview of the company and only then go into detail. This makes it easier for us to research, obtain structured information, and create clear documentation.
![[Pasted image 20260525121959.png]]
An important point to mention here, which may not be so apparent at first sight, is that there is no order for the information resources we need or must follow. This means that we have a great deal of flexibility to search through the different information resources and to adapt the information we find to our approach.

`Finally, a methodology is not a step-by-step guide but a structured workflow independent of the individual test cases.`

When we use OSINT, we can divide our core information results into three categories:

|**`Company Information`**|**`Infrastructure`**|**`Leaks`**|
|---|---|---|
|Organization|Domain Information|Archives|
|Locations|Public Domain Records|Internal Leaks|
|Staff|Domain Structure|Breaches|
|Contact Information|Cloud Storage||
|Business Records|Email Addresses||
|Services|Third-Parties||
|Social Networks|Compounded Social Networks||
||Technologies in Use||

In this methodology, we take a point from the information categories (`Core Elements`) and search for the relevant information for it through the different `information resources`.

Theoretically, the reverse procedure can be used too. We could filter out the different information resources and enter the corresponding information results into our documentation information areas. This is the most common method used by most people for OSINT. However, it has a significant disadvantage because we have many `information resources` to adapt our methodology to, rather than our information resources to our methodology. The result is that we are left with an unstructured approach and are guided by the information resources but not by the information results. We will explain this in more detail in a moment.

During OSINT, we will come across many different resources, which contain information not only for one category but also for others. Therefore we should have at least `two separate browsers` open.

## Workflow

`It is essential to understand that, using this methodology, we adapt our information resources to the methodology, not the methodology to our information resources.`

As a simple example, we can imagine that we want to search for all employees of a target company. Most people would start looking for the company on social networks like LinkedIn, Xing, and others, look at the company's homepage, and maybe even start a Google search. As mentioned before, this is the method that most people use, and therefore we base our methodology on the information resources.

However, if we follow our methodology, we will work step by step through the information resources shown above in cycles, starting, for example, with the home page, moving on to the social networks, using various search engines and development platforms and forums. In this way, we work according to an organized structure and get far more results.

It requires a little more effort to go through the same page several times to cover different information areas and categories (`Core Elements`). However, we maintain a structured and transparent methodology without overlooking specific details relevant to a particular information area and category. This also allows us to create clear and detailed documentation and work in cycles independent of the cases.

To make this structured methodology efficient, we need to work with two browser windows.

#### `1. Research Browser`

We use this one only for our research. Here we send all search requests and log the entire OSINT process. The research browser's history must be cleaned before use to prevent us from logging results from other companies. We can use the add-on called [History Master](https://github.com/jiacai2050/history-master) to log our searches and finally export them as documentation.

#### `2. Resource Browser`

The resource browser serves as a summary of the information resources we find during the investigation. In this one, we move all the information resources to which we can return to search for information for other categories. For example, we will get to know development platforms containing leaks and names of employees or developers, email addresses, and usernames.

In this part, we can also use the add-on called [SingleFile](https://addons.mozilla.org/en-US/firefox/addon/single-file/?utm_source=addons.mozilla.org&utm_medium=referral&utm_content=search). This allows us to copy the web pages with the information and save them locally as proof.

The actual process is quite simple, as we now split our results between `two` browsers. So as soon as we have found a new information resource and examined it in the `Research Browser` and discover that there is more useful information there, we drag the newly opened tab to the `Resource Browser`, which we will turn to later.

Next, let us assume we are working on the section/phase to look for email addresses. It looks like this, but it can also expand to other information resources.
![[Pasted image 20260525122015.png]]
We study the `home page`, `social networks`, use the `search engines`, `developer platforms`, and the `leak resources` if we are going to follow this structure. Once we have gathered the relevant information, the `cycle` starts again in this phase, and we begin again with the home page, social networks, etc., but this time with the new information we have found from the `first cycle`.

`We document all our discoveries and structure them accordingly to each phase and our cycle.`

If we find valuable information on an information resource that we want to document, we should download this web page accordingly with `SingleFile` and save it. We will find in the beginning that our documentation (if done correctly) will be far more than 50 pages longer than most OSINT reports that are usually delivered to the client.

We also know that the employer is mainly interested in the results. However, this is not a reason not to make our documentation as detailed as it is. For the employer, the short documentation at the beginning will be sufficient. However, if the employer then wants to remove the found/leaked information, the concise documentation will not be adequate by far. Here, the information resource and the procedure for how this information resource was found are of great importance.

## Logging

To work efficiently and in a structured way, we must also document the results and information we find clearly. However, we also need to log our steps and prove how we found the information. With the two different browsers, we have found a way to separate our search from the resources. To create clear documentation, we need three components:

1. Visited websites
2. Timestamp
3. Queries

All of this information is stored in our browser history, and we can use it quite efficiently for our purposes. A handy add-on for this is [History Master](https://github.com/jiacai2050/history-master). This add-on gives us many different options, like sorting, filtering, searching, displaying, and exporting statistics.
![[Pasted image 20260525122057.png]]
To work with history efficiently, we need a few more packages that will allow us to display the results better and make them easier to filter. If we have exported our history from `History Master` and look at it, it will look something like this:

#### History CSV
  

crimsonguard@htb[/htb]`$ head history.csv  visit-time,title,visit-count,typed-count,id,url 1607535086780,Contact – Inlanefreight,1,,tgIKscfGm4PL,https://www.inlanefreight.com/index.php/contact/ 1607535085390,News – Inlanefreight,1,,6S_PX7_woFpV,https://www.inlanefreight.com/index.php/news/ 1607535084030,Offices – Inlanefreight,1,,UNWCj5EDwP2e,https://www.inlanefreight.com/index.php/offices/ 1607535081962,Career – Inlanefreight,1,,6r25bcvsVT21,https://www.inlanefreight.com/index.php/career/ 1607535044507,Inlanefreight,2,,6bhPbKqKh9He,https://www.inlanefreight.com/ 1607535044304,https://inlanefreight.com/,2,,Od54NGgg3cyS,https://inlanefreight.com/ 1607535043995,http://inlanefreight.com/,2,,sfBT401xyE7c,http://inlanefreight.com/`

We can install the appropriate packages as follows:

#### Install NPM, JQ, and Csvtojson

crimsonguard@htb[/htb]`$ sudo apt install npm jq -y && sudo npm install -g csvtojson`

This would help us sort our output much better, allowing it to be filtered with `jq`, an incredibly powerful JSON processing command-line tool. We will pipe that gathered data into `jq` to print the JSON appealingly for now.

#### Sorted History View

crimsonguard@htb[/htb]`$ csvtojson < history.csv | jq .   [                                                          {      "visit-time": "1607535086780",     "title": "Contact – Inlanefreight",     "visit-count": "1",                                      "typed-count": "",      "id": "tgIKscfGm4PL",     "url": "https://www.inlanefreight.com/index.php/contact/"   },                                                       {      "visit-time": "1607535085390",     "title": "News – Inlanefreight",     "visit-count": "1",                                      "typed-count": "",      "id": "6S_PX7_woFpV",     "url": "https://www.inlanefreight.com/index.php/news/"   },                                                       {     "visit-time": "1607535084030",     "title": "Offices – Inlanefreight",     "visit-count": "1",     "typed-count": "",     "id": "UNWCj5EDwP2e",     "url": "https://www.inlanefreight.com/index.php/offices/"   },    <...SNIP...> ]`

#### SingleFile

We can store all the websites locally using the [SingleFile](https://addons.mozilla.org/en-US/firefox/addon/single-file/?utm_source=addons.mozilla.org&utm_medium=referral&utm_content=search) add-on from which we have found the relevant information for each category. This gives us the possibility to search web page contents locally as well as proof for our documentation. This add-on offers an excellent way to download all open tabs with one click. This means that we no longer have to worry about individual screenshots of the pages and do not have to do it manually for each page.
![[Pasted image 20260525122132.png]]
Another great advantage of this is that these stored pages can also be searched locally by our customers with simple terminal commands to see which web page the corresponding information is located on.

crimsonguard@htb[/htb]`$ cat *.html | html2text | grep "Emma Williams"  Emma Williams  emma.williams@inlanefreight.com`

This add-on also offers the possibility to save visited websites automatically. However, as we do not know what information the websites we visit will contain, this is not recommended. Of course, this can also be used for the log if necessary.

Important Note: In this module, a real company is used to illustrate efficiency based on real situations. Therefore, many graphics in the next sections are mostly censored in order not to put this company in focus. It is also strictly forbidden for students to use this module's content to trace the selected company and do further research against it or its personnel.
# Business Investigation

During the investigation of a company, we collect all possible data about it. In the process, we work our way from the rough to the detailed. Business Investigation is divided into three categories of information that we can obtain:

|`Company Information`|`Infrastructure`|`Leaks`|
|---|---|---|

We should note down or at least keep in mind some questions that will help us get a clear and efficient overview during our research. These questions will also help us establish links between the individual pieces of information that the actual `intelligence` piece of the phrase `OSINT` stands for.

## Company Information

Company information includes the company's general overview. This means we will try to understand the company's structure:

- How many employees does the company have?
- What is its objective?
- How is the company positioned in society and the market?
- How profitable is the company?
- How does the company function?
- How do they manage their tasks?
- What services does the company provide?
- How is the company positioned financially?
- Which target group does the company pursue?
- Where is the company located?
- What are the physical security measures?
- How do interaction and advertising with (potential) customers take place?
- How strong is the company's reputation?

This information provides us with an excellent overview of the company, enabling us to identify different companies' interests. We can then assess very precisely the level of damage a breach of the company's infrastructure would cause.

The process of staff investigation, such as employees, supervisors, and team leaders, can provide us with valuable information that allows us to assess their knowledge and experience. It is a very time-consuming stage to search for these people, as we first have to find all the people employed by the target company and then try to find everything relevant about them if the signed contract allows it.

In this search, we try to determine:

- What position do the employees hold?
- Which departments exist within the company?
- Their day-to-day tasks.
- What are they responsible for?
- What dependencies do the employee have?

Here we will only deal roughly with the individual employees. We will deal with them more directly in another Module called `OSINT: Staff Investigation`. It requires a slightly different approach to work with the information in an efficient and structured way. Once we have gathered this information, we will move on to profiling, which we will deal with more extensively in the `OSINT: Staff Profiling` Module.

Social networks are used for personal profiles and the sharing of information from one's own private life. They are also used to publish products and news that provide new information about the company and its technologies. Therefore, in this phase, we try to determine:

- Which products are being developed?
- Which technologies are utilized?
- Who are the developers?
- Which conferences do the employees attend?
- Where are these products used?
- Who uses these products and services?

For example, when new software is released and sold to thousands of companies, it is interesting to get a demo of that software and analyze it for vulnerabilities. If we can identify a vulnerability, all companies that use this specific software version will also be affected by the vulnerability.

## Infrastructure

For the infrastructure domain, we move into more technical details. Here we try to find out:

- How is the company set up in terms of information technology?
- Who are the administrators?
- Does it meet the best possible security standards?
- Which technologies are used?
- What entries are there about the domain?
- Which certificates can be obtained?
- How many domains are registered to the company?
- What is the ASN?
- Which netblocks has the company reserved?
- Which third-party providers are used, and for what?
- How many and which servers are publicly accessible?
- How many and which email addresses are available?

This gives us a much better understanding of the technologies and the technical environment we have to deal with. Understanding how the entire company is positioned from an infrastructure standpoint is essential for identifying potential attack vectors. For example, specific configurations in the DNS servers can give us a rough idea of how experienced our target company's respective administrator is.

## Leaks

Most of the data put on the internet is stored there for decades and can be found easily. We can find different versions of the web servers and the website's design, for example, or documents removed years ago that contained up-to-date information about employees, technologies, or processes.

Internal leaks can be found on various forums. When a developer on StackOverflow asks a question to solve a problem in their code, the code is often shared with the others to give better insight into their issue. If the developer is under a lot of pressure or distracted, they may accidentally post code containing passwords or other sensitive data. The functions that the developers are working on can be much more interesting. These may contain vulnerabilities if inexperienced developers are employed to write specific sections of code.
# Organization

When we talk about company information, we focus our interest on the points that can give us a detailed view of the entire business and its processes. This type of essential information is usually found on the target company's websites. We will not necessarily find all the necessary information to give us an `insight` into the company and its `processes`. This could tell us which companies will be most interested in working with our target company and which requirements must be met. Understanding our target company's business processes can give us an idea of how to structure our attacks. Therefore we can design our attacks to be executed during a specific step in the process.

We can also get an insight into the company's dependencies. From this, we will be able to conclude which technologies are needed to manage the company, which will indicate how the company may be structured from an infrastructure perspective.
![[Pasted image 20260525122209.png]]
Generally, the company's home page, social networks, and search engines can be used to map out the company's organizational structure. There are no useful tools that can be used to map out the company's organizational structure efficiently. Instead, we must rely on the logical association between the information we will find during OSINT and the actual intelligence.

Furthermore, it is challenging to keep it dynamic because every company has unique staffing needs and employees, all of whom bring different strengths, weaknesses, and abilities. Therefore this field of OSINT is more a repeatable process than a static method.

## Locations

If our engagement is a red team assessment, then such information is much more relevant than a regular penetration test procedure. Red-teaming can also include, among other things, the physical security of the company and is used to determine which methods and techniques can be used to obtain highly sensitive information that may not be accessible from the internet. The company's locations are of great importance for this. However, the scope and rules for which procedures may and may not be used must be strictly observed.

Companies already pay a lot of money to attract potential customers by presenting themselves to their customers in the best possible way and sell themselves better from a marketing point of view. Here we can always ask ourselves when the company would arouse our `interest`.
![[Pasted image 20260525122235.png]]
## Staff

Every company's marketing department attaches great importance to the best possible representation of its `staff` so that potential customers can be sure they will be taken care of in a professional manner. The company's employees handle all the processes. We are interested in the information they use to interact with the company and its infrastructure. This may include but is not limited to email addresses, phone numbers, usernames, passwords, and the social networks on which they operate.

The employee's role in the company can also be used to assess their privileges in specific areas. Thus, a manager would likely have higher rights than customer support. However, even if a secretary does not have direct access to the systems, the individuals in these roles can be an attractive attack vector for us since they most likely have full access to calendars, plans, contact data, email addresses, and more.
![[Pasted image 20260525122248.png]]
## Contact Information

Another critical point for all potential customers is the company's accessibility. For this reason, `contact details` are always disclosed. After all, customers may want to find out more about the company or even arrange a meeting. Contact information includes phone numbers and email addresses, and usernames from various communication portals such as Skype, Microsoft Teams, Slack, Discord, etc. These usernames can also be associated with the employees. This part of OSINT will be discussed in more detail at `OSINT: Staff Investigation`.
![[Pasted image 20260525122259.png]]
## Business Records

Significant customers look at the company's website and the `business records`, to learn more about it. From an OSINT perspective, they can also tell us a lot about its progress. These include the company's `locations`, `financial` situation, `references`, and `reputation`. We are particularly interested in poor feedback about the company.

Poor feedback requires poor communication and the resulting `failures` in the `business processes`. If a customer's inquiry or request is not fulfilled, it may not always be a human mistake on the part of the employee but may indicate `technical issues`. Companies often try to "hide" this feedback not to create a wrong impression for potential customers.

Business records also include the degree of recognition in the market. For example, we can get this through reviews by (former) employees or on social networks. Customer satisfaction also plays a role. We can conclude how structured and coordinated the company works internally to complete its services and overcome customer problems for the services provided.

The company's financial situation tells us a lot about its commitment and productivity. If a company continually offers new products and services, it can positively affect its financial situation. Depending on the company's size, the downside of this is that it can lead to chaotic processes. This can mean a great deal of organizational effort and communication. Therefore, it leads to a higher risk of phishing attacks because the content of a detailed, customized phishing email is rarely actually checked.
![[Pasted image 20260525122316.png]]
## Services

`Services` are also explained in great detail on the website so that the number of external inquiries is kept to a minimum. For this reason, how each service is carried out is usually discussed in detail. All other questions that arise are generally specific.
![[Pasted image 20260525122327.png]]
## Social Networks

We can also ask where to find more information about the company during a phone call. We are usually directed to `blog posts`, `videos`, or `documentation` from a marketing perspective that would give us better insight into the company. We can find a variety of information about the target company through the different social media networks. We usually first direct our focus to the company's linked accounts via its home page. In turn, these may lead us to different sources of information not mentioned on the home page.

We can also search the different platforms themselves to see where the company is represented on social networks. Finally, we can use search engines or forums to find out if the company is mentioned there.
![[Pasted image 20260525122341.png]]
# Locations

When we look into a company's locations, we have to identify the most obvious locations in advance. Larger companies that are active in the field of production sometimes have sites that they do not mention. These can also be identified in the OSINT process. The most prominent locations are usually listed on the company's home page. After all, the company wants to attract potential customers with the number of locations it has. It is an overall strategy for companies to present themselves as the best all over the world. This contributes significantly to marketing and increases the number of customers and thus the turnover.

In general, we can find basic information about a company's locations in advance using the following resources:
![[Pasted image 20260525122355.png]]
## Home Page

If we start with the home page and browse through the site a little, we will find `countries`, `cities`, `addresses`, and possibly even `maps` that show the company's locations.
![[Pasted image 20260525122417.png]]Most of the time, we will also find lists that include the countries and/or cities where the company has its offices. These locations are usually managed locally by administrators but also may be managed centrally. If the company is centrally managed, these locations (either the entire company or specific locations) within the scope can also provide us with attack vectors that we can use to get into the systems' central administration infrastructure.
![[Pasted image 20260525122428.png]]
## Social Networks

Social networks are used to share much information, especially information that potential customers can use to learn more about the company. This includes the publication of locations of offices or production facilities. This type of information can be found on many social networks such as [Twitter](https://twitter.com/), [LinkedIn](https://www.linkedin.com/), [Instagram](https://www.instagram.com/), [Snapchat](https://www.snapchat.com/), [YouTube](https://www.youtube.com/), [WeChat](https://www.wechat.com/en/), [Facebook](https://www.facebook.com/), and others. Most links to a company's social networks can be found on their home page.
![[Pasted image 20260525122438.png]]
If we look through the posts on Twitter, we can find the company's locations and even potential routes that they use. By looking at the routes, we can determine how long specific people will be on the road, which may help us calculate the time it takes to become active as penetration testers.
![[Pasted image 20260525122450.png]]
Regarding the countries that the company transits, we know that it is international cooperation, which suggests that most business is conducted in English. Therefore employees may be using American or UK English keyboards and not a Chinese keyboard layout. This is valuable information to generate possible passwords when targeting employees in an attempt to gain access to externally exposed services such as email or VPN.

## Search Engines

Every company `location` can provide us with another potential attack vector, whether physical or technical. Each site has different employees and managers who have their way of working and can open up different attack paths for us than other locations. As soon as we have detailed information about the locations, we should document them and, in the best case, add a graphical representation as evidence in our documentation. For example, we can use [Google Maps](https://maps.google.com/), [Google Earth](https://earth.google.com/web/), [ShowMyStreet](https://showmystreet.com/), and many others to obtain these types of views.
![[Pasted image 20260525122509.png]]
In this case, we could pretend to be a significant client for potential collaboration and organize a meeting to be let into the building and get a much better insight into the company's processes. If the contract requires a physical test, we can also get an inside view of the building's physical security, such as door locks, windows, security, cameras, rooms, departments, and more.

Information about locations is mainly interesting for `physical Red Team` operations. This is because it distinguishes between the times at which a "break-in" can be most efficient. If a physical assessment is conducted during working hours, we must be granted appropriate access to restricted areas upfront. If possible, it is better to find a way to enter the building at night if the alarm systems and their vulnerabilities allow it. The locations themselves are the company's buildings and the location of the `offices`. Sometimes there are also badge readers that we can see and use to identify how to set up our cloner to hijack access cards wirelessly.

Suppose a manager or even higher employee feels safe in their office, which has only access to a minimal number of employees and suddenly finds a USB stick on their desk. In that case, they may wonder where it came from. The feeling of being safe increases the likelihood that the USB stick we leave behind will be plugged into their computer.

In larger companies, we will find some of these locations, which at first glance already indicate some potential weaknesses in the field of physical security. Suppose we have such an assignment where we also have to test the organization's physical security. In that case, we can use the same resources to get a picture of the terrain or even physically walk through the location as a "passerby" and record a video of the terrain with a simulated phone call. Depending on whether the location and the terrain allow it, we can also take photos from a distance. By using the online resources, we will also be able to gain some information, as shown below:
![[Pasted image 20260525122523.png]]
However, we have to keep in mind that these photos have not been made today and may not be up to date. Nevertheless, if we take a closer look at them, we can already see some interesting characteristics of the building, which could be helpful for us.
![[Pasted image 20260525122533.png]]
We see on the second floor that there are windows that can be opened from the outside. The black pipes on the roof are metal air ducts. Theoretically, these can be used as a clamping aid for climbing up with the help of a grappling hook if a simple ladder is not available. The insight into the building itself also shows the door mechanisms that were installed. This plays a role when we want to access restricted areas when entering the building, like the manager's office, as we have previously assumed as an example.

If we take a closer look at the building from the left side, we will discover some more important information.
![[Pasted image 20260525122545.png]]
Here we recognize three other vital parts of the building. First, we see an elevator that may be useful to us. This type of place is an excellent spot to "drop" a USB stick that we have configured, which others can then pick up. We should never underestimate a person's curiosity. If we label the USB stick with "Confidential," it will increase the interest of the people who find it. Confidential material is only ever intended for certain groups of people, and the desire to know something "secret" is far stronger than just an openly presented object. Furthermore, we see the same door mechanism to the right of the staircase. This confirms that the second floor is not a closed area but that this mechanism was most likely installed on all doors in the building.

The tilting window has blinds, which on higher floors are generally used to protect against the sun. These rooms can be valuable and may be computer rooms or server rooms which are of great interest to us. Another interesting aspect is that we can determine the width and height of the building. Building regulations dictate how specific parts of a building can be designed and must be built in most countries. Also, the bricks have standard dimensions, which we can use to determine the dimensions. Now let us look at the building from the back.
![[Pasted image 20260525122600.png]]
We can see an antenna, another entrance into the building, and an installed camera at the back of the building. The second entrance is also attractive for us because it offers another possibility to enter the building. The antenna could become interesting for us if no camera is installed on the side of the building. A lot of information can be gained from these types of photos. Finally, we use these photos as the basis for scenarios that could lead to someone entering the building. If we are required to test physical security, we will make recommendations to close such potential gaps, which could lead to an intrusion.

---
## Questions

What are the city's coordinates where one of the company's offices, "inlanefreight.com" has its headquarters in Germany? We suggest to use https://latitude.to for this.
- google: inlanefreight office locations Oberhausen and DD Coordinates
- https://latitude.to/map/de/germany/cities/oberhausen
- 51.47311 6.88074

What are the city's coordinates where one of the company's offices, "inlanefreight.com" has its headquarters in United Kingdom? We suggest to use https://latitude.to for this.
- 50.82838 -0.13947

What are the city's coordinates where one of the company's offices, "inlanefreight.com" has its headquarters in USA? We suggest to use https://latitude.to for this.
- 39.73915 -104.9847

In which country is the chief financial officer located?
- Germany

How many locations does the company have in total?
- 14

---

# Staff

Information about the company's employees and managers is essential for us, enabling us to reconstruct the internal corporate structure. With this information, we will be able to identify the different departments' managers and design our social engineering attacks better and more efficiently. We are not yet looking at individual employees, but rather looking at who is employed, who is in charge of what, who is responsible for what, and try to get a picture of the internal communication flow.

The methodology we have used so far can also be applied to staff. However, this field covers many potential attack vectors for `phishing campaigns` and `social engineering` attacks, which we need to filter out. We will get many valuable results, but this does not cover everything applied to the staff. This field will be discussed in more detail in the Module `OSINT: Staff Investigation`.

In this step, we should continue to focus on getting an overview of the company's employees to gain insight into its internal structure and hierarchy and to be able to reconstruct it if necessary.

There are many sources we can use to obtain this information. Among them are the following:
![[Pasted image 20260525133401.png]]
## Home Page

Most of the time, we can find the company's essential contact persons on the "`About Us`" page. These staff members are usually trained to present themselves professionally to customers and have the necessary answers to the most critical questions.
![[Pasted image 20260525133422.png]]
It is interesting to note that we can read a biography for the C-level staff here. This can serve us very well later in social engineering to find common topics we can discuss and gain a certain amount of trust and sympathy from the relevant persons. An important thing to note here is that these people would only deal with large customers who promise a significant profit. Otherwise, it can be hard to get directly to them.

We are interested in the employees who are already employed and want to know what types of employees the company is actively recruiting. IT offers many different specializations, enabling us to identify which technologies the company works with or plans to work with by reading through open job requisitions. Therefore we can continue to search for job offers on the homepage that are still open or on the company's [LinkedIn](https://www.linkedin.com/) profile.
#### Job Offers - HomePage
![[Pasted image 20260525133456.png]]
## Files

Often companies offer different flyers, reports, and news in the form of `files`. The software used to create these files is usually registered and only available to authorized employees registered in the system. This data also includes the software that labels the created files with metadata. These can then be extracted and read out with the help of `exiftool`. This information can include usernames, dates, software and version numbers, geographical coordinates, and much more. We should download as many accessible files as possible and extract the metadata accordingly.

#### Report.pdf

crimsonguard@htb[/htb]`$ exiftool Report.pdf   ExifTool Version Number         : 11.88 File Name                       : Report.pdf Directory                       : . File Size                       : 226 kB File Modification Date/Time     : 2020:12:07 16:33:00+01:00 File Access Date/Time           : 2020:12:07 16:32:59+01:00 File Inode Change Date/Time     : 2020:12:07 16:34:16+01:00 File Permissions                : rw-rw-r-- File Type                       : PDF File Type Extension             : pdf MIME Type                       : application/pdf PDF Version                     : 1.5 Linearized                      : No Page Count                      : 2 Language                        : en-US Tagged PDF                      : Yes Title                           : Lorem ipsum dolor Author                          : John Doe Creator                         : Microsoft® Word 2016 Create Date                     : 2020:12:03 13:42:50+01:00 Modify Date                     : 2020:12:03 13:42:50+01:00 Producer                        : Microsoft® Word 2016`

## Social Networks

Finally, most employees do not want to miss the opportunity to get better job offers from other companies and keep their profiles updated on `business networks`. Maybe even with the latest projects they had to deal with to show off their skills. Every company wants to have the best possible presence on social media and, therefore, publishes links to their `company profiles`. These are often also linked to the profiles of all their `employees`. One such source could be [LinkedIn](https://www.linkedin.com/), for example.

#### Employees - LinkedIn
![[Pasted image 20260525133530.png]]

Apart from the individual employees and their profiles, LinkedIn can also give us a better overview of our target company. For example, we can find out an estimated number of employees for our target company.

#### Employees - Number
![[Pasted image 20260525133544.png]]
This means that we have just over 8,000 employees officially linked to the company. If our scope allows it, we would have over 8,000 potential attack vectors that we could use to attack the company network via a phishing attack.

#### Job Offers - LinkedIn
![[Pasted image 20260525133557.png]]
On [LinkedIn](https://www.linkedin.com/), we can also enter specific keywords in the search field, which will filter the target company employees to make our search more specific. We need people who work on computers and typically are not interested in those that do not use a computer at all in their day-to-day jobs.

#### Employees - IT Department
![[Pasted image 20260525133608.png]]
Finally, we have filtered out our potential targets very well and have specific ones to focus our attention. All business and social networks will be covered in a later section. Other valuable sources for employees and job positions are:

||||||
|---|---|---|---|---|
|[Xing](https://www.xing.com/)|[Monster](https://www.monster.com/)|[Indeed](https://www.indeed.com/)|[Glassdoor](https://www.glassdoor.com/)|[TrustPilot](https://www.trustpilot.com/)|

However, to make it a little bit clearer how effective OSINT can be and how much data people disclose, check out the following video:

![](https://youtu.be/sq-0tjv4_BA?si=-RWdkn6h4D0B5PAe)

Furthermore, we can use `search engines` we know from which we will discover other employees, the social networks on which they appear, the `development platforms` used by developers, and other files from which we can extract metadata. We can obtain further information through these `internal leaks`. The search for employees requires an adapted approach and represents an additional larger field of information sources. It is crucial to examine them separately. Otherwise, we might get stuck in this phase. This will become much clearer in the `OSINT: Staff Investigation` Module.

---
## Questions

Answer the question(s) below to complete the section

Check the website www.inlanefreight.com and find out the name of the chief operating officer and submit his full name as the answer.
- Max Cartmoon

How many positions does the company Inlanefreight want to have filled in the future?
- 15

How many logistics and software specialists does the Inlanefreight company employ at least?
- 40

---
# Contact Information

Contact details may be relevant to contact the appropriate person and send inquiries to find out more about the company's processes. These may include technical issues where we want to ensure that the products we offer will work with our working environment.
![[Pasted image 20260525134313.png]]
## Home Page

If the company wants to present itself and clarify individual needs in the best possible way, the `contact details` page on their website will list people from different departments.

#### Contact - Website
![[Pasted image 20260525134326.png]]
For example, in the next Penetration Testing phase, we could call the `phone numbers` to pretend to be customers of the company, ask short technical questions, and ask them to help us with our issues. It is also interesting to note that larger companies often have more than one domain. Therefore, the contact persons' `email addresses` may be on a different domain than the one on which the main website is located. We could, for example, use these to prepare our phishing campaigns and use these email addresses as targets. Phishing attacks can provide us with sensitive information, such as passwords or usernames, which can be very helpful, if not essential, in our penetration testing process.

This information is crucial for us, especially for phishing, vishing, and social engineering attacks in the `Exploitation phase`. Employees from the customer service and sales teams communicate a lot with (potential) customers and therefore easily fall into a routine requiring repetitive processes. By human nature, routine operations reduce the attention span, leading to the inattentive and careless performance of their tasks. This is one of the weaknesses that we can use for our purposes when we talk to an employee and influence them to click on a link we send him.

## Social Networks

Social networks provide us with a platform for interacting with the company's employees and offer a wide variety of information about them. It is irrelevant whether these are private social networks or business networks. What is important is that we can obtain additional information through each of these networks, including `contacts`, `partners`, `co-developers`, their `phone numbers`, and `email addresses`.
![[Pasted image 20260525134339.png]]
This information is rarely made available to the public. However, with an accepted connection request, we can obtain valuable information and talk directly with the person to find more details.

## Search Engines

It is also beneficial to note down all the persons' `names` because they have to log in to their domain with the respective identifiers or unique name format. IT people are especially interesting because most companies allow IT people to do their work from home. This indicates that there must be a specific type of remote access mechanism for connecting to the internal network remotely.

Another excellent source to find out the email addresses of the target domain is [Hunter.io](https://www.hunter.io/).

#### Email Addresses - Hunter.io
![[Pasted image 20260525134351.png]]
We can verify each email address via [Emailrep.io](https://emailrep.io/) and check how the mail servers handle it.

#### Verify Email Address - Emailrep.io

crimsonguard@htb[/htb]`$ curl emailrep.io/<firstname>.<lastname>@<domain>.<tld>  {   "email": "<firstname>.<lastname>@<domain>.<tld>",   "reputation": "high",   "suspicious": false,   "references": 7,   "details": {     "blacklisted": false,     "malicious_activity": false,     "malicious_activity_recent": false,     "credentials_leaked": false,     "credentials_leaked_recent": false,     "data_breach": false,     "first_seen": "never",     "last_seen": "never",     "domain_exists": true,     "domain_reputation": "high",     "new_domain": false,     "days_since_domain_creation": 5130,     "suspicious_tld": false,     "spam": false,     "free_provider": false,     "disposable": false,     "deliverable": true,     "accept_all": true,     "valid_mx": true,     "primary_mx": "mx1.mailcontrol.com",     "spoofable": true,     "spf_strict": true,     "dmarc_enforced": false,     "profiles": []   } }`

[RocketReach](https://rocketreach.co/) allows us to identify email address formats by domain and the other email addresses linked to the corresponding managers. It is a paid service provider, but the results we get from it are very accurate, and it is an excellent way to find out links to the company's employees and the platforms they use.

#### Email Format
![[Pasted image 20260525134405.png]]
#### Linked Email Addresses
![[Pasted image 20260525134414.png]]
#### Google Search

We can also use simple Google search queries to make further connections to employees from other platforms. We must keep in mind that some social media networks are not known worldwide and are only used in specific countries for local purposes. In the process, we will repeatedly come across these platforms, which may contain helpful information. An example of one such platform is [AllPeople](https://allpeople.com/).
![[Pasted image 20260525134426.png]]
However, we must not forget to use other search engines such as [Bing](https://www.bing.com/), [Yahoo](https://yahoo.com/), [DuckDuckGo](https://duckduckgo.com/), and others. Each search engine works differently, and accordingly, we get different results, links, and orders for our queries. Here is a list of the most used search engines:

||||||
|---|---|---|---|---|
|[Google](https://www.google.com/)|[Microsoft Bing](https://www.bing.com/)|[Yahoo](https://yahoo.com/)|[Baidu](https://www.baidu.com/)|[Yandex](https://yandex.com/)|
|[DuckDuckGo](https://duckduckgo.com/)|[Ask.com](https://ask.com/)|[Ecosia](https://www.ecosia.org/)|[Aol.com](https://www.aol.com/)|[Internet Archive](https://archive.org/)|

## Leak Resources

When we talk about leaks, we are not just talking about published databases that contain users' passwords and email addresses. Apart from the fact that these databases offer an excellent research opportunity, if we know the domain to which the email addresses are registered, we can also find new and unknown email addresses.

Apart from that, `WayBackMachine` from [Archive.org](https://web.archive.org/) offers us a very effective way to search for contact information. It shows us older versions of the company website when IT security was less in focus than today. Before a company becomes large, the bosses establish themselves much more in all the necessary processes to efficiently control them. Therefore, email addresses were often published on the websites at that time to establish contact with the managers and customers better. However, in most cases, these email addresses are rarely changed by bosses and highly-positioned members. Therefore, we need to review snapshots of the target company website on WayBackMachine as part of the OSINT process. Below is an example of the various snapshots taken for a sample target company. Browsing to one of these would show us the state of the company website on the given date, which may differ significantly from the current website version and provide us with useful information or even expose hidden functionality.

#### WayBackMachine - Snapshots
![[Pasted image 20260525134442.png]]

---
## Questions

Answer the question(s) below to complete the section

Check the website www.inlanefreight.com and find out the email address of John Smith and submit it as the answer.
- john.smith4@inlanefreight(dot)com

What is the email address for enterprise customer support?
- enterprise-support@inlanefreight(dot)com

---
# Business Records

Business records are also of great importance to us, as they show us how the company is doing in the market and how successful it has been to date. If there are significant fluctuations, we can then focus on the individual events and determine which factors brought about the change. In this way, we can understand in retrospect what impact specific actions related to these factors may have had on the company. Finally, our clients want to know how it can financially harm their company. A detailed overview that identifies these factors can represent an accurate prognosis of the financial damage resulting from a potential intrusion.
![[Pasted image 20260525134712.png]]
## Home Page

`Financial records` can give us a good overview of how the company is doing and how it is currently growing. It also gives us information about what the company depends on and whether it is presently healthy from a financial standpoint. As soon as a company is under high pressure, it naturally tries to save as much money as possible. These types of financial constraints often force companies to seek out lower-quality services from third parties. It does not matter whether these services are technical or not. With lower quality services, it is more likely that they do not meet the highest security standards and that we will be able to find some information about our target company.
![[Pasted image 20260525134724.png]]
## Files

Files that we can find on the internet contain not only potentially important metadata but also general information. Every company must present itself efficiently and profitably. After all, they want to attract new customers and show their success to convince them to give them their business. Many companies demonstrate their success through financial reports. These can be found on the company's website or are sent to subscribers via newsletters.
![[Pasted image 20260525134735.png]]
Using Google search, we can find and filter out files like .docx, PDFs, and others. We can use `Google Dorks` for these purposes. A small list of the most important "dorks" can be found [here](https://www.recordedfuture.com/threat-intelligence-101/threat-analysis-techniques/google-dorks).
![[Pasted image 20260525134756.png]]
## Social Networks

In addition to finances, the company's culture also plays an essential role. This enables us to determine how certain situations are handled and how reliably the processes function. Suppose a 3rd-level support staff member is neglected and frustrated with their company's management concerning their position. In that case, it is a very strong demotivator that consequently reflects on their performance, attention, and attentiveness. This employee will most likely put in far less effort and even less thought into the company's future.

With the help of [Google](https://www.google.com/), we can find more reviews and reports about the company. This will help us identify where the most difficulties and problems occur in their processes that we can focus on later. It is essential to note the perspective from which the `reviews` have been written. They can be written either by `customer` or an `employee`. When we Google the reviews, we will find sources such as [Indeed.com](https://www.indeed.com/), which gives us an excellent opportunity to look at the reviews to get a better idea of the atmosphere and process.
![[Pasted image 20260525134814.png]]
## Search Engines

On [Crunchbase.com](https://www.crunchbase.com/), we can find some of the `published records` concerning our target company. Often we can find financial reports from which we can find more helpful information. These reports also list some information about employees.
![[Pasted image 20260525134828.png]]
If our customer is a publicly-traded corporation, it is even easier for us to track its financial status. This is because we can view detailed information on them on [Yahoo! Finance](https://finance.yahoo.com/).
![[Pasted image 20260525134840.png]]
For us as penetration testers, it is enormously important to look behind the scenes and `think outside the box`. We need to understand how the company's financial situation has developed and which `data` and `factors` have contributed to this. If we know these, we can start to trace their `management processes` and go into more detail to understand what steps were necessary and how we as "attackers" could influence them by penetrating the corporate network.

---
## Questions

Answer the question(s) below to complete the section

Investigate the website www.inlanefreight.com and find out how much EBIT they recorded for the third quarter of 2020 and submit it as the answer. (Format example: USD 000,000,000)
- USD 276,000,000

---

# Services

We must `think outside the box` regarding a company's services and understand how the background processes are set up. To get a better overview, we should understand the company's goals surrounding its services for its customers and itself.
![[Pasted image 20260525135017.png]]
Another valuable indirect source that can give us information about the technical equipment of the target company is its partners, when little information is available about these services, as shown in the following example.

## Home Page

On the product and service descriptions page, we will often find step-by-step instructions that describe the entire process between the customer and the completion of the service or the receipt of the product. Utilizing options that are sometimes provided, such as the localization of the representative offices and contact persons, we can also find, among other things, tools that the website visitor can use. In the following example, we can see where the company's stops are on its shipping routes.

#### Service Finder
![[Pasted image 20260525135032.png]]
In this case, we can see that we can find out the specific shipping routes and that they form a network. These services may include information about the `technologies`, their `workflow`, `employees` who take care of these services, and `resources` that may contain potentially security-sensitive information.

#### Service Options
![[Pasted image 20260525135045.png]]
Suppose we are not familiar with the industry but want to find out how these processes and orders are set up and managed. In that case, we should search through the internet to find out what options are available and look at `similar providers` who may provide additional information. We must remember that all companies in the same industry are usually competitors. At the same time, the desire to be the #1 in the marketplace brings the same desire to provide the `best possible services` to customers, which are often `very similar`. In many cases, the company also takes advantage of `service gaps` that the competitors do not or only poorly fulfill.

These are then often prioritized. This service is presented with much more information to show that this company is significantly better than the competition.

#### Competitors Service Descriptions
![[Pasted image 20260525135055.png]]
Nowadays, the management of services is done almost exclusively through a form of software application. A high focus is placed on `web applications` and `mobile applications`, making operation and control as easy as possible for customers.

#### Service Applications
![[Pasted image 20260525135106.png]]
Here we can find information about the software not only from the descriptions but also from videos. They can give us an impression of the features in advance for operating elements that require user input.

#### Service Applications - Resource
![[Pasted image 20260525135116.png]]
We can also find out about different functions of these applications that our target company may not have mentioned by analyzing the company's competitors.

#### Service Applications - Competitors App Features
![[Pasted image 20260525135135.png]]
## Files

Information about the target company's `partners` can also be found `in files`. This is because not all partners and technologies are presented and disclosed directly. However, these companies may be marked as "`Powered by`" in these files.

#### Technologies
![[Pasted image 20260525135145.png]]
As mentioned earlier, we can already see that our target company uses `cloud solutions` offered by the provider. This is also very valuable to us, as we now know that we should be on the lookout for potential cloud-based storage locations that may be publicly accessible.

#### Technologies - Powered by
![[Pasted image 20260525135155.png]]
In addition to these files, information about services is often stored in detail in documents such as PDF files. This is often done because of the possibility to design these documents to be very visually appealing while communicating a lot of detailed information about the service. Here, too, appearance plays a significant role for the companies. After all, they always want to show that they are up to date and offer unique solutions for their customers' needs. It is precisely these self-developed solutions that provide us a potential attack vector that we could exploit.

#### Electronic Communication
![[Pasted image 20260525135205.png]]
## Social Networks

In principle, social networks are used by companies for marketing purposes to bring their products and services to the people and to draw attention to themselves. Here, too, information about their services and solutions is presented and published. In most cases, this is also linked so that these solutions can be viewed directly and the company's website is visited. This can play an essential role, especially with new releases of service solutions and applications. If this has been tested poorly (or even not at all) for security and vulnerabilities, then this application/solution represents a potential attack vector.
![[Pasted image 20260525135216.png]]
## Forums

A wide variety of technical discussions and news are discussed in forums. These are very useful for us when we want to find out something about a company's performance and services. Every post in the forum usually has a `date` when we find out when an enhancement or application, or service was released. Based on this time-lapse, we can roughly estimate how up-to-date specific `software` may be. For example, if we find an application that uses a `library` containing a vulnerability, we will be able to exploit it with the appropriate preparations and measures. More information about vulnerable libraries and how many applications are affected by those can be found in this [Veracode report](https://www.veracode.com/blog/research/announcing-our-state-software-security-open-source-edition-report).

Larger companies that have a large number of customers sometimes even provide forums for customers. We can often register and peruse these forums. `Technical problems` are usually discussed there, and we can also ask questions to find out more information. We can sometimes even see how the developers or administrators solve specific problems. With a little bit of advanced programming knowledge, we could even understand how a specific software-related problem could be solved.

---
## Questions

Answer the question(s) below to complete the section

Investigate the website www.inlanefreight.com and find out the name of the API application the company uses and submit it as the answer.
- InlaneConnect

How many liners does the company own in total?
- 72

---
# Social Networks

With social networks, things can now get a bit more complicated and confusing, as they serve as both information elements and sources of information. If we look at social networks as sources of information, we enter what is known as `Social Media Intelligence` (`SOCMINT`). To not lose the orientation and overview of this, during the `OSINT` phase, we focus on the "movement" and presence of the company's social networks. Therefore, in this case, we will deal with the social networks and information elements. Once we get an overview of the company's presence on the social networks, we can move into `SOCMINT` and examine and analyze each platform in more detail.
![[Pasted image 20260525135641.png]]
To make the distinction clearer, we use social networks as `Information Elements` (marked green) rather than `Information Sources` (marked red) in this phase. This means that we are collecting the company's connections to social networks.

However, this does not mean that we have to limit ourselves here. We can combine both, but we have to keep in mind that we consider social networks as information elements (i.e., pure information for the company's value). We can find a lot of information about the company itself and its technologies. These include public and internal information but is not limited to:

|`Blogs/News`|`Images & Videos`|`Wikis`|`Documents & Files`|`Social Media`|
|---|---|---|---|---|

## Home Page

Almost every medium-sized company strives to keep current and potential customers up to date. In most cases, these sources are also straightforward to find. After all, this `news`, which is also often published on `blogs` and `social media` platforms, provides information about our target companies' progress and success. This information is often provided to convince potential customers that the company is always the best choice.

#### Blogs and News
![[Pasted image 20260525135653.png]]
This news often includes new cooperations with partners and successes achieved by the company. Additionally, new technologies and solutions for customers are presented here that we should also consider. Soon we will look at a document that may contain this type of information.

## Social Networks as Information Component

Most companies see social media platforms as essential and indispensable from a marketing point of view. In most cases, we find the links to the various platforms directly on the website itself. On these social media platforms, such as [Twitter](https://twitter.com/), [LinkedIn](https://www.linkedin.com/), [Xing](https://www.xing.com/), [Facebook](https://www.facebook.com/), [Instagram](https://www.instagram.com/), [YouTube](https://www.youtube.com/), and others, we find not only the `latest news` and `developments` of the company but also older posts that can give us information about the `structure and technologies`.

Another often overlooked component, which does not directly belong to the social media category but fulfills the same purpose, is `newsletters`.
![[Pasted image 20260525135703.png]]
## Search Engines

All search engines also allow us to search for images and videos of the company, which can also provide us with links to social networks and information sources.

#### Images & Videos

Companies often use `images` and `videos` to increase the attractiveness of blog posts and news articles. In the last section, we have already seen that we could identify a mobile application from a video and get a short impression of how it looks. Now let's look at a picture of our target company that provides us with some information.

#### Office
![[Pasted image 20260525135716.png]]
Depending on the age of the image, certain information about the target company's technologies may vary.

|**Marked Red**|
|---|
|However, we can see in this picture in the `marked red` area that the operating system `Windows` is used. If we take a closer look at it, we see some kind of `messenger` in the lower right corner and many small `icons` that indicate the applications used. If the image's resolution was even better, we could also read the `application name`, which we can use to search for known vulnerabilities. We could also get either the `real name` or the sender's `username` from the messenger.|

|**Marked Green**|
|---|
|In the `marked green` area, we can see that this is a `ThinkPad`. We can see that by the `two red markings` on the mousepad. If we compare the screens' picture, we can see that `Windows` is installed on the `ThinkPad` too.|

|**Marked Blue**|
|---|
|The `marked blue` area contains a `wireless mouse`. Since we know that these devices can also be exploited from over 100 meters away, we should also note this valuable piece of information.|

|**Marked Yellow**|
|---|
|In the `marked yellow` area, we can see that our target company uses `Excel`. We do not get more detailed information about the Excel version, but we know it is used with Excel's corresponding `file extensions`.|

`Photos` offer a far more significant security risk than most people realize. Let us say we find a high-resolution photo of a company meeting where the work `IDs` are visible. Especially for `red-team operations`, this is incredibly beneficial because we can use the photo to recreate and prepare a badge to get past the building's security personnel and get inside the company's building.

#### Documents

The search for documents in this section is based on the fact that it allows us, among other things, to refer to sources on which social media platforms these files are stored.

A good start for finding documents and connections is Google. Using `Google Dorks`, we can define parameters that should be displayed. For this part, it is enough to know that we can use the Google Dork "`filetype:`" to define the filetype that we are seeking. This results in output to links that explicitly point to the specified file type of our target. Accordingly, we can download and view them.

Do not forget that we must document `each step` and the corresponding `source` we use to find the information. Otherwise, we will have difficulty describing how and where we got this information from in the report.
![[Pasted image 20260525135737.png]]
Of course, we can and must also use other file types here. We will see later in the section `Internal Leaks` the valuable information these files can give us, which can play a crucial role in the success of our engagement.

## Forums

As mentioned earlier, forums serve as a good source of information for technical difficulties and problems. Problems are also discussed and dealt with on social networks. There are countless public forums such as [Reddit](https://www.reddit.com/), [StackOverflow](https://stackoverflow.com/), and others that the company can use for this purpose. Here, too, we focus mainly on the presence of forums where people discuss our target company.

Internal forums that we may stumble upon are very interesting to us. In these, technical questions are answered in far greater detail than in public forums. Therefore, we should keep an eye out if we can perhaps register on an internal forum to search for information.
![[Pasted image 20260525135747.png]]
#### Wikis

The exciting thing about wikis is the information that is published and the `references` and `external links`, which in turn point us to other sources that can provide us with even more information. Some references and resources can be [Wikipedia](https://en.wikipedia.org/), [Github](https://github.com/), or even `internal/provided wikis` to which it can be linked.

We can find a lot of helpful information from the wikis that describes the company itself or its services. Wikis are created to provide detailed information about specific topics and make them accessible to others. In other words, they often serve `as documentation` that can provide valuable information for us.

For example, we may find a post describing a specific application in detail (i.e., how to install it, how to work with it, and more.) We can determine which dependencies these applications have and can later use this information for `Reverse Engineering` to understand the developer better and uncover potential weaknesses.

---
## Questions

Answer the question(s) below to complete the section

How many social networks are shown on the website of the company Inlanefreight?
- 4
---
# Domain Information

We use domain information to get a general idea of how the company is structured from a technical standpoint. It is essential to understand the basics of network technology, which we have discussed in Module [Introduction to Networking](https://academy.hackthebox.com/course/preview/introduction-to-networking) to be able to dig into how a company is structured on a technical level.

Here we work our way piece by piece from `rough to detailed` information. Therefore we first have to get a `technical overview` of the company. Since we usually already know the name of the company or even the domain name, this information is generally sufficient to determine how the company is structured.
![[Pasted image 20260525135840.png]]
It is important to note that the illustration above is only the generalized illustration that applies to most companies and scenarios and should not limit us. Each company is `individually` set up and structured, which can lead to the fact that we can get the required information via the other information resources to which the arrows in this illustration do not point.

We can classify the rough structure into three categories:

|`Public Records`|`Third Parties`|`Domains`|
|---|---|---|

We need to keep this phase's goal in mind since we will be coming across many different sources and resources. After all, we are only now beginning to get an overview of the company's infrastructure. We have to be careful not to go into detail for now while we work through the following three categories, but to get an overview of how the company is positioned on the Internet.

The element we focus on in the first phase of the company's infrastructure investigation is the domain names we can find ([SLD](https://en.wikipedia.org/wiki/Second-level_domain) and [TLD](https://en.wikipedia.org/wiki/Top-level_domain), i.e., "target-company.com"). After all, these are the primary nodes on which the company's presence on the Internet is based. We then go into each domain and get an overview of the subordinate structure that contains `Netblocks`, `Name Servers`, `Mail Servers`, `subdomains`, and `hosts/IP addresses` for each domain.
![[Pasted image 20260525135855.png]]
#### Public Records

Public Records are the registrars with which the company has registered the corresponding domain. Since we work from rough information to detailed information, we can use the domain name to determine which `IP ranges` the domain is located in. Based on this, we can determine which `IP addresses` may belong to the company. This includes the corresponding `DNS servers` and `Mail servers`, which can later be used for attacks.

Other (`sub`)`domains` are also included. Different administration and marketing areas are shared to manage the corresponding customer groups more efficiently in most cases.

#### Third Parties

Another critical point is `third parties`. Here we have to pay close attention to what assets belong to these third parties because, during our penetration tests, we will need permission from the third party to test their systems or services if they provide services for our client. Here we have to clarify `precisely` which assets belong to these third parties to not accidentally attack them.

This includes `hosting providers` but also the `cloud` and others. If the target company has cloud assets or the website runs via a hosting provider, we `must` request the corresponding permission from the third-party provider from our client. The client must inquire with the third-party provider, inform them, and obtain the appropriate consent for us to test their systems soon.

Otherwise, this could have `serious legal consequences` for us as well as for the client.

#### Domains

Information about a domain can give us many attack vectors. Mainly because a company rarely registers just a single domain. Administrators often become complacent and pay less and less attention to details which makes our work easier.

However, it is essential to pay attention to what was defined by the client in `the scope`. Since we are `always in contact` with the client and contact them directly in case of critical discoveries, we can also suggest extending the scope accordingly if we suspect particular vulnerabilities in another domain. How this is handled, however, depends entirely on the client.

Nevertheless, as good penetration testers, we should always act in the client's best interest and provide the best possible service.

## Social Networks

We often find references to `domains` or `subdomains` on social media platforms. As we already know, the companies' latest technologies are presented there, and other news is shared. Social media platforms are all aimed at different target groups. Some are more for the general public, and others are for business purposes. Nevertheless, some platforms cover both groups. The dissemination of information is also very much geared to the goal that the company is pursuing. This affects how and where information is shared.

After all, a product or service that is intended for large companies does not necessarily make sense. This is not necessarily true, but this example should only serve as an illustration of the differences. After all, this depends on many factors, such as target group, marketing strategy, product orientation, financial possibilities, and much more. Furthermore, `hashtags` are often a beneficial information element that allows us to narrow down relevant information about our target company on the entire platform (or even across social media platforms).
![[Pasted image 20260525135911.png]]
The images we find on these platforms are usually not intended for only one of them. They are often uploaded to other platforms as well. We can find them with a `reverse image search` like [TinEye](https://tineye.com/) or [Google Images](https://images.google.com/) and get additional sources of information that we can use.

## Search Engines

One of the most efficient methods of searching for `domains` is offered by various `search engines`. In this case, we cannot focus on a domain name, but we have to work with general company terms, such as the company name, the name of the application or service, and others. We must try to narrow down the search results to our target company to avoid misinformation that costs us valuable time. Finally, we need to pay attention to how these (at best unique) terms and labels we use are scattered on the Internet and how they connect us to our target company.

When Googling these terms, we can stumble across a wide variety of information resources. Each of the discovered and visited information resources should be saved in our research browser for later examination.

Another excellent way to find out information about our target domain is to use a particular `Search Engine Optimization` (`SEO`) field called `backlinks`. A backlink is a link from an external page to another website. Search engines like Google use the backlink profile as an indicator to rank the website. The more high-quality backlinks, the more popular a page appears to be on the Internet. The advantage of backlinks analyzers is that they aggregate all backlinks, identifying the sources that discuss and mention our target company. One of such backlinks analyzers is [Ubersuggest](https://app.neilpatel.com/en/seo_analyzer/backlinks).
![[Pasted image 20260525135923.png]]
The interesting thing here is that we can see all the sources and identify the webserver structure and its contents (paths, files, subfolders) without performing any active enumeration.

From the beginning, Google has focused on the evaluation of backlinks as a signal of trust between domains. This orientation has made Google the most powerful and relevant search engine. The famous PageRank calculation, which formed the basis of Google's search algorithm, analyzes links between different pages on the Internet and deduces how trustworthy a particular website is.

## Development Platforms

Development platforms also offer excellent information resources for us here, as we will often find code that sometimes even provide information that is dangerous for the company. This information can range from `IP addresses` and `hostnames`, configuration files to `credentials`. These platforms include, but are not limited to:

|[Github](https://github.com/)|[GitLab](https://gitlab.com/)|[Google Code](https://code.google.com/)|[Bitbucket](https://bitbucket.org/)|
|---|---|---|---|

Again, we can search for the terms like the company name or the application name they offer. Although the developers know what data might be stored there, this is often considered low-risk. Besides, `configuration files` or even `backups` can also be found. This type of data is not infrequently encrypted, but even if it is, it does not prevent us from downloading them and trying to decrypt them.

Author's note:

During every investigation of a company where I found sensitive information this way, the most common excuse given by the responsible employees was that no one cared. Nevertheless, I was obligated to immediately contact the customer if I found information such as credentials and recommended that they remove them as quickly as possible. There is no justification from the administrator's or developer's point of view to keep this information unrestricted. The only thing to do is change the leaked credentials immediately and set the resource with this information to "private" if it can not be taken down. In the end, the developers and the administrators could see that this small piece of information allowed me to compromise the company's servers and would have caused massive financial damage.

## Leak Resources

Once we have a list of the company's `domains`, we can use it to look for known anomalies that affect the company and the corresponding domain. One of the best sources of information for this is [VirusTotal](https://www.virustotal.com/), where we can scan each domain for suspicious activity.
![[Pasted image 20260525135938.png]]
These entries can point to `files`, `email addresses`, `domains`, and other links. Searching on the developer platforms goes hand-in-hand with the leak resources, and it will help us a lot to keep track of things by looking for the related `domains` first and looking at the other information later.

Another source that searches against many different developer platforms is [Searchcode](https://searchcode.com/). It searches all possible codes for terms that we specify in the search and shows us the sources accordingly. It is not uncommon for us to come across information that may look something like this:

#### Sourcecode
![[Pasted image 20260525135948.png]]
#### Github
![[Pasted image 20260525135959.png]]
# Public Domain Records

As we know, we need a rough overview of the technical structure of the company. After we have identified and filtered out all the domains that belong to them, we can focus on their structure and composition.
![[Pasted image 20260525140014.png]]
Public domain records also offer us excellent opportunities to trace the company's information technology infrastructure structure. With the right arrangement and knowledge of what the records are for and what information they contain, they can provide us with information about the company's Internet presence. To get this, we need to find out at least four components:

|`1. Netblocks / CIDR`|`2. ASN`|`3. DNS Servers`|`4. Mail Servers`|
|---|---|---|---|

This gives us an overview of which systems are `accessible` from the internet, in which `address range` they are located, which `IP neighbors` they have, and how the interaction between them takes place.

## Search Engines

When going through different search engines, we are talking about the search engines that can be used for this purpose and databases containing relevant information for us. The minimum information we need to get from our client, apart from the company's name, is a domain or at least an IP address to start with. The scope of this can vary greatly.

#### 1. Netblocks / CIDR

During a black-box penetration test, our client will often only provide the domain name. We can use this to find out a lot of helpful information. First of all, we should find the `IP address` of the main webserver(s) and the `IP address range(s)` / `CIDR`. For this, we can use the following command.

#### Host

crimsonguard@htb[/htb]`$ host www.inlanefreight.com  www.inlanefreight.com has address 134.209.24.248 www.inlanefreight.com has IPv6 address 2a03:b0c0:1:e0::32c:b001`

Now that we have the IP address of the web server, we can find out which `IP address range` it is located in. By default, we can work with the `Whois` protocol used by a distributed database system to retrieve information about internet domains and IP addresses and their owners.

#### Whois

crimsonguard@htb[/htb]`$ whois 134.209.24.248  % IANA WHOIS server % for more information on IANA, visit http://www.iana.org % This query returned 1 object  refer:        whois.arin.net  inetnum:      134.0.0.0 - 134.255.255.255 organisation: Administered by ARIN status:       LEGACY  whois:        whois.arin.net  changed:      1993-05 source:       IANA  # whois.arin.net  NetRange:       134.209.0.0 - 134.209.255.255 CIDR:           134.209.0.0/16 NetName:        DIGITALOCEAN-134-209-0-0 NetHandle:      NET-134-209-0-0-1 Parent:         NET134 (NET-134-0-0-0-0) NetType:        Direct Allocation                                                OriginAS:       AS14061` 

We can also search the `WHOIS` databases for the company name. We are often given different identification numbers (`mnt-ref`), which we can use for further searches. These stand for the maintainer objects, which are used as a reference for the organization objects.

#### WHOIS - MNT-REF

crimsonguard@htb[/htb]`$ whois -B --sources RIPE,ARIN target-company  % Information related to 'ORG-ZZ000-RIPE'  organisation:   ORG-ZZ000-RIPE org-name:       Target-Company LLC country:        CO org-type:       XXX address:        Street 00 address:        00000 address:        City address:        COUNTRY phone:          +00000000000 fax-no:         +00000000000 mnt-ref:        EU-IBM-XXX-0000 mnt-ref:        HN0000-MNT mnt-ref:        AS0000-MNT mnt-ref:        RIPE-XXX-ZZ-MNT mnt-by:         RIPE-XXX-ZZ-MNT mnt-by:         HL0000-MNT abuse-c:        AHA000-RIPE created:        2010-01-12T13:57:44Z last-modified:  2021-10-14T12:14:22Z source:         RIPE e-mail:         full.name@target-company.com  % Information related to 'AHA000-RIPE'  role:           ABUSE Target-Company LLC address:        Street 00, 00000 City, Country e-mail:         dns.admin@target-company.com abuse-mailbox:  dns.admin@target-company.com nic-hdl:        AHA000-RIPE mnt-by:         RK00000-MNT created:        2014-11-15T11:54:10Z last-modified:  2014-11-15T11:54:10Z source:         RIPE  % Information related to 'HN0000-RIPE'  role:           Target-Company NOC address:        Street 00, 00000 City, Country e-mail:         dns.admin@target-company.com nic-hdl:        HN0000-RIPE mnt-by:         AA00000-MNT created:        2019-04-07T06:44:22Z last-modified:  2019-04-07T06:44:22Z source:         RIPE`

We can also query the individual databases and identify the associated netblocks.

#### WHOIS - Netblocks

crimsonguard@htb[/htb]`$ whois -h whois.arin.net target-company | grep -v "#" | sed -r '/^\s*$/d'  Target-Company (C00000) 10-10-60-68 (NET-10-10-60-68-1) 10.10.60.68 - 10.10.60.69 Target-Company (C00000) 10-10-60-70 (NET-10-10-60-70-1) 10.10.60.70 - 10.10.60.71`

Here is the [ARIN list](https://www.arin.net/resources/registry/whois/rws/cli/) of other flags we can use to get more information from the maintainer objects.

#### 2. ASN

We can also see another crucial piece of information here, the `OrigisAS`. This is the `Autonomous System Number` (`ASN`). This number is unique and is made publicly available so routing information can be exchanged with other systems. This is done with specific IP prefixes, which we can use to find out the netblocks (`public ASNs`). There are also `private ASNs` intended for systems that only communicate via a provider. Other protocols such as the `Border Gateway Protocol` (`BGP`) are used if this is the case. We can use [MXToolbox](https://mxtoolbox.com/SuperTool.aspx) and its `ASN Lookup` option with the ASN to find out how many subnets the owner has.
![[Pasted image 20260525140036.png]]
Another way to get more information about the domain is to use the [ICANN lookup](https://lookup.icann.org/lookup). Each domain is registered to an organization or person with a unique ID, which provides information about when the domain was created and expires.
![[Pasted image 20260525140046.png]]
#### 3. DNS Servers

DNS servers are essential services today because they help the regular user reach the web services they want. We are now familiar with how DNS works after we have worked through `DNS Enumeration using Python` Module and know what information we can get from it. It is often the case that companies own several domains, and accordingly, these domains can offer different attack vectors that can affect each other. The next step is to look at the DNS records of our target domain using `dig`. `Dig` is a DNS lookup utility that can be used to obtain publicly available information from DNS.

#### DNS Records - Inlanefreight.com

crimsonguard@htb[/htb]`$ dig any inlanefreight.com  <SNIP> ;; ANSWER SECTION: inlanefreight.com.	7191	IN	A	X.X.149.43 inlanefreight.com.	7191	IN	MX	10 cluster-j.inlanefreight.com. inlanefreight.com.	7191	IN	MX	10 cluster-e.inlanefreight.com. inlanefreight.com.	7191	IN	TXT	"v=spf1 include:inlanefreight.com ?all" inlanefreight.com.	7191	IN	TXT	"nFM+YEvc/npla0It+GWBy9tZgud2Z3Q2i/bFxrHRfbzv1G09Mx7pB9==" inlanefreight.com.	7191	IN	TXT	"MS=ms80134" inlanefreight.com.	7191	IN	SOA	ns1.inlanefreight.com. abuse.infreight.com. 2012110855 21600 3600 604800 600 inlanefreight.com.	7190	IN	NS	ns1.inlanefreight.com. inlanefreight.com.	7190	IN	NS	ns2.inlanefreight.com. <SNIP>`

If we now take a closer look at the records, we see that the SOA record shows another domain, `infreight.com`. Therefore we will also look at these if the scope allows us to do so. In this case, we assume that we have permission to test this domain as well.

#### DNS Records - Infreight.com

crimsonguard@htb[/htb]`$ dig any infreight.com  <SNIP> ;; ANSWER SECTION: inlanefreight.com.		7200	IN	A	134.209.24.248 inlanefreight.com.		7200	IN	SOA	ns1.infreight.com. adm.infreight.com. 2012111147 21600 3600 604800 600 inlanefreight.com.		7200	IN	MX	10 cluster-e.inlanefreight.com. inlanefreight.com.		7200	IN	MX	10 cluster-j.inlanefreight.com. inlanefreight.com.		7200	IN	TXT	"MS=ms90101" inlanefreight.com.		7200	IN	TXT	"v=spf1 include:mailcontrol.inlanefreight.com include:mailing.inlanefreight.com ?all" inlanefreight.com.		7200	IN	TXT	Here is the [ARIN list](https://www.arin.net/resources/registry/whois/rws/cli/) of other flags we can use to get more information from the maintainer objects:"8MdY05nra9NlO2voN5icWjuhq0za+5JvpM3xAgt/gg==" inlanefreight.com.		7200	IN	NS	ns3.inlanefreight.com. inlanefreight.com.		7200	IN	NS	ns1.inlanefreight.com. inlanefreight.com.		7200	IN	NS	ns2.inlanefreight.com. <SNIP>`

Again, we have discovered new potential targets based on the DNS records only, which we have to document in our list to take a closer look at them later.

Another source we can use, which gives us a much better representation of the DNS servers' records, is [DNSdumpster](https://dnsdumpster.com/).

#### DNSdumpster - Records
![[Pasted image 20260525140101.png]]
Here we can see the entries for the respective DNS servers and the `locations` of the corresponding hosts and servers.

#### DNSdumpster - Locations
Here we can see the entries for the respective DNS servers and the `locations` of the corresponding hosts and servers.

#### DNSdumpster - Locations
![[Pasted image 20260525140121.png]]
Then we see the respective IP addresses and corresponding subdomains that could be obtained from the entries. Additionally, [DNSdumpster](https://dnsdumpster.com/) can sometimes show us the services running on the server if it can passively identify them.

#### DNSdumpster - Hosts
![[Pasted image 20260525140131.png]]
Finally, at the bottom of the page, we can see a domain map that shows the relationships between the servers connected with the records.

#### DNSdumpster - Domain Map
![[Pasted image 20260525140141.png]]
#### 4. Mail Servers

Mail Server / Exchange server (`MX`) is one of the most critical services today, as it ensures that our emails reach the desired communication partners. Other servers use it as an interstation (relay) for sending spam or viruses. It often happens that `MX` servers do not only use specially registered servers as relays. This gives us the possibility to perform an `Open Relay Attack`.

`MX` servers represent a massive attack vector. There is nothing worse for a company apart from a complete compromise than the fact that all unencrypted emails can be intercepted and read. This means that not only GDPR guidelines have been violated by requiring the company to ensure that customer data is kept confidential, but it also significantly impacts customer satisfaction and customer trust, which will be significantly damaged.

[MXtoolbox](https://mxtoolbox.com/) offers an excellent service to test the `MX` servers for Open Relay.

#### MXtoolbox - Open Relay Check
![[Pasted image 20260525140157.png]]
## Development Platforms

Searching for code on developer platforms can bring surprising results. Apart from already highly sensitive data such as user names and passwords, we can also find `configuration files` containing the latest administration settings. We can use [Searchcode](https://searchcode.com/) again with specific strings and known `IP addresses` or `domain names` to get such results. However, in this case, we need a basic understanding of the configuration files, how they can look, and which variables can be used for DNS configuration.
![[Pasted image 20260525140207.png]]

---
## Questions

Answer the question(s) below to complete the section

Find out how many nameservers are responsible for the inlanefreight.com domain and submit the number as the answer.
- 2

Find out the FQDN of the mail server of the inlanefreight.com domain and submit it as the answer.
- mail1.inlanefreight.com

What is the registry domain ID of inlanefreight.com?
- 2420436757_DOMAIN_COM-VRSN

What is the name of the registrar of this domain?
- Amazon Registrar, Inc.

Examine the DNS records and submit the TXT record as the answer.
- HTB{5Fz6UPNUFFzqjdg0AzXyxCjMZ}

---
# Domain Structure

Now that we have gathered a lot of information, we need to focus on the `intelligence` of this process and create an `overview of the domain`, and `analyze` our results. We should take our time because the better we understand the structure of the domain, the easier it will be to take the appropriate steps later. Furthermore, we narrow down the goals and seek out the `low hanging fruit` of all the systems that promise the best possible results for us.
![[Pasted image 20260525142706.png]]
This preparation can take several hours of study but save us time in the following steps because we will not test each system blindly, but `proceed methodically`, `organized` and `structured`. This represents the professionalism of our work and gives a much better impression in the reports for our customers.

## Home Page

Countless websites link to other subdomains. This may be marketing or may have been preconfigured for analysis reasons. Therefore, we should always pay attention to each clickable area on every web page owned by the company.
![[Pasted image 20260525142716.png]]
#### Subdomains

The search for subdomains is multifaceted, and many methods can be used for this purpose. These methods are widely discussed, and many of these techniques are presented as legal. Here we have to be very careful because only the `first four` techniques are legal and do not directly interact with the target's infrastructure (provided we have permission).

1. `Certificate Transparency`
2. `Forward DNS Datasets`
3. `Web Search`
4. `Disclosure / Leak`
5. Subdomain Brute-Forcing
6. AXFR

`Subdomain brute-forcing` is an active testing method where a domain query is sent to the DNS servers. Any dynamic interaction and investigation of the domain without written permission is `illegal` and may result in criminal charges. If any damage is done in the inquiry, these queries can be tracked very easily.

`AXFR` also belongs to an active method that refers to the misconfiguration of a DNS server. This method can have even `more severe consequences`, as a targeted attempt is already being made to exploit a potential vulnerability.

Therefore, we should `always pay attention` and always check what we do next, and whether it is in line with the contract signed with our client.

## Search Engines

Since we are still in the passive information gathering phase, we should use passive techniques to find out more subdomains. For this, we can use a tool called [CTFR](https://github.com/UnaPibaGeek/ctfr). It uses `Certificate Transparency` logs from [Certificate Transparency](https://www.certificate-transparency.org/) and [crt.sh](https://crt.sh/).
  
Note: The command output seen below displays fictionalized subdomains, meant to serve as an example for what a user may or may not see when using the `ctfr.py` tool.

crimsonguard@htb[/htb]`$ ./ctfr.py -d inlanefreight.com | grep -v "[-]"            ____ _____ _____ ____            / ___|_   _|  ___|  _ \          | |     | | | |_  | |_) |         | |___  | | |  _| |  _ <           \____| |_| |_|   |_| \_\     Made by Sheila A. Berta (UnaPibaGeek)  inlanefreight.com vc.inlanefreight.com afdc0002.inlanefreight.com afdc0101.inlanefreight.com afdc0102.inlanefreight.com wlan.inlanefreight.com afdc0102.inlanefreight.com wlan.inlanefreight.com autodiscover.inlanefreight.com kfdcex07.inlanefreight.com kfdcex08.inlanefreight.com videoconf.inlanefreight.com  [!]  Done. Have a nice day! ;).`

We can now use [CTFR](https://github.com/UnaPibaGeek/ctfr) for each subdomain in a `For-Loop` because there may be other subdomains in the respective subdomains. After we have collected and documented all passive results, we can use a simple `For-Loop` in Bash to determine the corresponding IP address for each subdomain.

crimsonguard@htb[/htb]`$ for sub in $(cat subdomains);do host $sub | grep "has" | cut -d" " -f1,4;done  inlanefreight.com X.X.149.43 vc.inlanefreight.com X.X.164.24 autodiscover.inlanefreight.com X.X.145.34 kfdccv04.inlanefreight.com X.X.48.249 ftp.inlanefreight.com X.X.48.249 rbpoint.inlanefreight.com X.X.48.249 <SNIP>`

Then we can use `whois` and `ipcalc` to find out the IP ranges for the respective IPv4 addresses.

#### IP Addresses

There are many different resources we can use to find out the IP addresses of our target company. One of the best and most used resources is [Shodan](https://www.shodan.io/). `Shodan` also offers a [CLI](https://cli.shodan.io/) version that we can install. This allows us to query, filter, and save the results directly from the command line, making documentation much more manageable.

#### Shodan

crimsonguard@htb[/htb]`$ shodan domain <TARGET-DOMAIN> | grep -w "A" | cut -d"A" -f2 | cut -d" " -f7 | sort -u > IPv4s.txt`

crimsonguard@htb[/htb]`$ cat IPv4s.txt | wc -l  36`

Now we can see that we were able to find `36` unique IPv4 addresses that we can later analyze and check for vulnerabilities. Of course, only if they are within the scope and not otherwise specified in the contract. If we run all found IPv4 addresses through `Shodan`, we will determine which ports the hosts have open in a passive way without having to interact with them directly.

crimsonguard@htb[/htb]`$ for ip in $(cat IPv4s.txt);do shodan host $ip;done  X.X.164.24 Hostnames:               vc.inlanefreight.com Country:                 United Kingdom Organization:            Inlanefreight Updated:                 2020-07-23T15:26:40.055514 Number of open ports:    7  Ports:     443/tcp      2222/tcp      5060/udp TANDBERG/4137 (X12.5.1)     5222/tcp      7001/tcp      7443/tcp      8443/tcp    X.X.48.186 Hostnames:               mail.inlanefreight.com Country:                 United Kingdom Organization:            Inlanefreight Updated:                 2020-07-08T08:36:17.845819 Number of open ports:    2  Ports:     443/tcp      8888/tcp    <SNIP>`

After this, we can use [IPinfo.io](https://ipinfo.io/). This resource provides an excellent way to identify the subnets and hosting providers.

Good results are also offered by the previously discussed platform [VirusTotal](https://www.virustotal.com/). It is important to note that the datasets are collected from different sources by these platforms. Therefore, it is necessary to check both of them and complete the missing entries with each other.

#### VirusTotal
![[Pasted image 20260525142746.png]]
Another great way to quickly search for subdomains is [C99.nl](https://subdomainfinder.c99.nl/index.php). We should remember that we should always use multiple sources to find all subdomains if possible. This is because we will rarely have situations where a single source provides us with all available subdomains. Additionally, we can see here if the hosts are behind `Cloudflare` and are protected or not.
![[Pasted image 20260525142757.png]]
## Virtual Hosts

With the IP addresses and subdomains, we can determine how many and which subdomains are `virtual hosts` (`vHosts`). Some good sources that we can use are [Pentester-Tools](https://pentest-tools.com/information-gathering/find-virtual-hosts) and [Hacker-Target](https://hackertarget.com/).

#### Hacker-Target - vHosts
![[Pasted image 20260525142808.png]]
We can easily recognize `vHosts`, because they share the same IP address for at least two subdomains. As we can see from the previous example, we have three `vHosts` sharing the same server.

#### Manual vHosts Discovery

```shell-session
kfdccv04.inlanefreight.com X.X.48.249
ftp.inlanefreight.com X.X.48.249
rbpoint.inlanefreight.com X.X.48.249
```

Another possibility is to enter the IP addresses in a search engine. Most of the time, these are connected to different subdomains or even to other service providers that also run on the same server.

## Development Platforms

For the developer platforms, we should be on the lookout for all possible files, as eventually, any of them may contain hints about `domain names` or `IP addresses`. Configuration files are of particular interest, as they may contain access data and have fixed IP addresses and (sub)domains. For this, we can again use [Searchcode](https://searchcode.com/) to find files of this kind quickly.
![[Pasted image 20260525142822.png]]
Another very interesting source of information is the [SEOptimer](https://www.seoptimer.com/). This is an SEO analysis tool that examines and evaluates the entire website for individual components. The results include fields like:

- Sub-Pages
- SEO
- Rankings
- Usability
- Performance
- Social
- Security
- Technology Rankings
- Recommendations

In general, marketing tools are designed to evaluate visitors' interactions on the website and allow SEO specialists and web designers to make the appropriate adjustments to improve the rating or create a better UX. Since they work a lot with links, we will likely find out a lot of helpful information.
![[Pasted image 20260525142832.png]]
## Leak Resources

Leak resources are unauthorized publications of information. This term is broad and can therefore include many different information components. These resources also include databases with datasets containing information about our target company. At the end of 2017, Rapid7 started [Project Sonar](https://opendata.rapid7.com/sonar.fdns_v2/), which collects and stores responses forwarding DNS requests. DNS records such as `A`, `AAAA`, `CNAME`, and `TXT` lookups are stored at certain intervals in individual `GZIP` files in the form of `JSON`. These databases are extensive and can exceed 30GB in compressed format. They contain (sub)domains, record types, and the corresponding IP addresses. This information resource serves as an updated and valuable resource for us to understand our target company's domain better.

crimsonguard@htb[/htb]`$ pigz -dc rapid7-fdns-db.json.gz | grep "target-company"  {"timestamp":"...","name":"autodiscover.swp-target-company.de","type":"a","value":"10.125.65.10"} {"timestamp":"...","name":"booking.target-company-reisebuero.de","type":"a","value":"10.64.96.118"} {"timestamp":"...","name":"campaigns.target-company.com","type":"cname","value":"mnto-lon0000000.com"} {"timestamp":"...","name":"danzas-mig.target-company.com","type":"a","value":"10.9.149.36"} {"timestamp":"...","name":"danzas-rel.target-company.com","type":"a","value":"10.9.149.73"} {"timestamp":"...","name":"dev.target-company.foreverdigital.org","type":"a","value":"10.10.202.10"} {"timestamp":"...","name":"ecard.target-company.com","type":"a","value":"10.26.61.134"} {"timestamp":"...","name":"target-company-llc.at","type":"a","value":"10.9.149.49"} {"timestamp":"...","name":"target-company-llc.biz","type":"a","value":"10.9.149.53"} {"timestamp":"...","name":"target-company-buchen.de","type":"a","value":"10.237.138.44"} {"timestamp":"...","name":"target-company-christmas.de","type":"a","value":"10.160.13.20"} {"timestamp":"...","name":"target-company-christmas.de","type":"a","value":"10.160.15.20"} {"timestamp":"...","name":"target-company-container.com","type":"a","value":"10.9.149.53"} {"timestamp":"...","name":"target-company-cruises.com","type":"a","value":"10.10.64.166"} <SNIP>`

The advantage here is that we can discover internal IP addresses and new `TLD`s. This gives us a much larger attack surface if the scope in the contract with our customer allows this. Some domains are added for specific fields, which we can use as further information resources.
## Questions

Answer the question(s) below to complete the section

Investigate the website www.inlanefreight.com and find out the Apache version of the webserver and submit it as the answer. (Format: 0.0.00)

Submit task answer

What is the hosting provider for the inlanefreight.com domain?

Submit task answer

Show Hint

What is the ASN for the inlanefreight.com domain?

Submit task answer

On which operating system is the webserver www.inlanefreight.com running?

Submit task answer

How many JS resources are there on the Inlanefreight website?

Submit task answer