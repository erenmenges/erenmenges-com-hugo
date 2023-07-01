
---
title: "Analysis of Security Oriented HTTP Response Header Usage across Federal and Non-Federal US Governmental Organizations"
date: 2022-09-07
draft: false
author: "Eren Mengeş"
authorLink: "/about/"
summary: HTTP response headers are mechanisms that assist in improving security in websites. In this paper, 5088 domains were tested using seven different tests.
---
## Abstract

**	**
HTTP response headers are mechanisms that assist in improving security in websites (“OWASP”). While they are not the sole universal indicator of website security, they are important enough to take cognizance of. In this paper, 5088 domains were tested using seven different tests. Among all tested headers, the Subresource Integrity (SRI) header was the least properly implemented header type by 32%. Cross-Origin Resource Sharing (CORS) header was the most properly implemented header type with 99.6%. On average, websites with .gov domains belonging to tribal domains implemented HTTP headers and features least properly by 45.4%, and websites with .gov domains belonging to federal domains implemented HTTP headers and features best by 70.8%. Additionally, 17.1% of all tested websites still implemented the deprecated X-XSS-Protection header. 


## Introduction
**	**

HTTP response headers are mechanisms that assist in improving security in websites (“OWASP”). A website’s security is dependent on many factors, and it would be incorrect to judge security solely by evaluating HTTP response headers. Nevertheless, this paper measures the usage of 7 different HTTP response headers across websites belonging to federal and non-federal US governmental organizations. The proper implementation of redirection mechanisms is also evaluated.


## Methodology
**	**
Two datasets of United States (US) government-related domains that had the .gov top-level domains (TLD) were available on the US government's own DotGov website. One dataset only included the domains of federal organizations, and another dataset had all US .gov domains in it. 

A Python script was written to process the datasets. First, domains that did not correspond to a website and domains that were redirected to other websites were filtered. Then,  functioning domains that correspond to unique websites were scanned with Mozilla’s HTTP Observatory Python library. 

The final dataset was a CSV file with each line consisting of a .gov domain and its test results. The Python script could not determine some redirects of the domains; therefore, those domains were marked and later checked manually. These are the seven tests evaluated in this paper: Cross-Origin Resource Sharing (CORS), redirection, HTTP Strict Transport Security (HSTS), Subresource Integrity (SRI), X-Content-Type-Options, X-Frame-Options (XFO), and X-XSS-Protection. The GitHub repository hosting all the datasets including raw and processed datasets alongside the Python script, can be found in Appendix A.

All of these tests were conducted during August 2022.


## Results (in percentages)

**	**
<table>
  <tr>
   <td><strong>type</strong>
   </td>
   <td><strong>cross-origin-resource-sharing</strong>
   </td>
   <td><strong>redirection</strong>
   </td>
   <td><strong>strict-transport-security</strong>
   </td>
   <td><strong>subresource-integrity</strong>
   </td>
   <td><strong>x-content-type-options</strong>
   </td>
   <td><strong>x-frame-options</strong>
   </td>
   <td><strong>site avg</strong>
   </td>
  </tr>
  <tr>
   <td><strong>city</strong>
   </td>
   <td>99.8
   </td>
   <td>46.4
   </td>
   <td>22.5
   </td>
   <td>33
   </td>
   <td>54.6
   </td>
   <td>52
   </td>
   <td>51.4
   </td>
  </tr>
  <tr>
   <td><strong>county</strong>
   </td>
   <td>99.8
   </td>
   <td>54.3
   </td>
   <td>31
   </td>
   <td>34.3
   </td>
   <td>42.4
   </td>
   <td>43
   </td>
   <td>50.8
   </td>
  </tr>
  <tr>
   <td><strong>independent intrastate</strong>
   </td>
   <td>100
   </td>
   <td>59.8
   </td>
   <td>40.2
   </td>
   <td>22.5
   </td>
   <td>41.2
   </td>
   <td>36.3
   </td>
   <td>50
   </td>
  </tr>
  <tr>
   <td><strong>interstate</strong>
   </td>
   <td>100
   </td>
   <td>86.7
   </td>
   <td>46.7
   </td>
   <td>26.7
   </td>
   <td>33.3
   </td>
   <td>53.3
   </td>
   <td>57.8
   </td>
  </tr>
  <tr>
   <td><strong>state</strong>
   </td>
   <td>98.2
   </td>
   <td>55.8
   </td>
   <td>27.4
   </td>
   <td>27.7
   </td>
   <td>40.9
   </td>
   <td>47.3
   </td>
   <td>49.6
   </td>
  </tr>
  <tr>
   <td><strong>tribe</strong>
   </td>
   <td>100
   </td>
   <td>53.3
   </td>
   <td>22.5
   </td>
   <td>44.2
   </td>
   <td>30
   </td>
   <td>22.5
   </td>
   <td>45.4
   </td>
  </tr>
  <tr>
   <td><strong>federal</strong>
   </td>
   <td>99.5
   </td>
   <td>82.9
   </td>
   <td>86.5
   </td>
   <td>27.2
   </td>
   <td>55
   </td>
   <td>73.9
   </td>
   <td>70.8
   </td>
  </tr>
  <tr>
   <td><strong>nonfederal</strong>
   </td>
   <td>99.6
   </td>
   <td>49.5
   </td>
   <td>25.1
   </td>
   <td>32.7
   </td>
   <td>49.8
   </td>
   <td>48.7
   </td>
   <td>50.9
   </td>
  </tr>
  <tr>
   <td><strong>all</strong>
   </td>
   <td>99.6
   </td>
   <td>53.7
   </td>
   <td>32.9
   </td>
   <td>32
   </td>
   <td>50.5
   </td>
   <td>51.9
   </td>
   <td>53.4
   </td>
  </tr>
</table>



### Results by Headers/Features




![Bar Graph of Results by Headers or Features](/images/govdomains/results_by_header.png)



#### Cross-Origin Resource Sharing Header (CORS)

CORS is a system that allows a server to specify any domain, scheme, or port other than its own from which a browser should allow loading resources (“Cross-Origin”). The following cases were considered improper implementation:



* CORS with universal access,
* CORS files not parsable.

Every other case was considered proper implementation (“HTTP”).

99.6% of all tested .gov domains implemented CORS properly. The domain categories that properly implemented CORS the best were Independent Intrastates, Interstates, and Tribal domains by 100%, meaning all tested domains belonging to these domain categories implemented CORS properly. The domain categories that properly implemented CORS the worst were state domains by 98.2%. Overall, 99.6% of non-federal domains (consisting of city, county, independent intrastate, interstate, state, and tribe domains) implemented CORS properly.






![Bar Graph of CORS Results](/images/govdomains/cors.png "Bar Graph of CORS Results")



#### Redirection

Taking the user to website B when the user tries to connect to website A is called “redirection”. Whether the website redirects to its HTTPS version when the user tries to connect via plain HTTP was tested. Implementation of any of the following cases were considered proper implementation:



* Unable to connect via plain HTTP,
* Redirection to HTTPS page on the same host,
* No matter where the website redirects, the destination is in the HTTP Strict Transport Security (HSTS) preload list.

Every other case was considered improper implementation (“HTTP”).

53.7% of all tested .gov domains implemented redirection properly. The domain category that properly implemented redirection the best was Interstate domains by 86.7%. The domain category that properly implemented redirection the worst was state domains by 46.4%. As a whole, 49.5% of non-federal domains (consisting of city, county, independent intrastate, interstate, state, and tribe domains) implemented redirection properly.




![Bar Graph of Redirection Results](/images/govdomains/redirection.png "Bar Graph of Redirection Results")



#### HTTP Strict Transport Security (HSTS)

HSTS lets a website instruct the browser that it should never serve the site using plain HTTP and should instead revise all plain HTTP access attempts to HTTPS requests (“HSTS”). These cases were considered proper implementation:



* The website is present in HSTS preload list and loads via preloading,
* HSTS header set to no less than six months.

Every other case was considered improper implementation (“HTTP”).

32.9% of all tested .gov domains implemented HSTS properly. The domain category that properly implemented HSTS the best was federal domains by 86.5%. The domain categories that properly implemented HSTS the worst were city and tribal domains by 22.5%. As a whole, 25.1% of non-federal domains (consisting of city, county, independent intrastate, interstate, state, and tribe domains) implemented HSTS properly.





![Bar Graph of HSTS Results](/images/govdomains/hsts.png "Bar Graph of HSTS Results")



#### Subresource Integrity (SRI)

SRI is a mechanism that enables browsers to verify that resources they fetch from another host are delivered without manipulation. The website provides a cryptographic hash for every resource, and the browser then checks if fetched resources really have that same provided hash (“Subresource”). These cases were considered proper implementation:



* All scripts are from an identical source, and SRI is implemented,
* All scripts are loaded securely via SRI,
* All scripts are from an identical source, and SRI is not implemented,
* There are no scripts on website; therefore, no SRI is needed,
* Only HTML resources on websites need SRI.

Every other case was considered improper implementation (“HTTP”).

32% of all tested .gov domains implemented SRI properly. The domain category that properly implemented SRI the best was tribal domains by 44.2%. The domain category that properly implemented SRI the worst was independent intrastate domains by 22.5%. As a whole, 32.7% of non-federal domains (consisting of city, county, independent intrastate, interstate, state, and tribe domains) implemented SRI properly.




![Bar Graph of SRI Results](/images/govdomains/sri.png "Bar Graph of SRI Results")



#### X-Content-Type-Options Header

The X-Content-Type-Options response HTTP header is a marker used by the server to enforce that the MIME types provided in the Content-Type headers should be followed strictly. Proper implementation of this header prevents MIME sniffing (“X-Content-Type-Options”). This case was considered proper implementation:



* “nosniff” value in X-Content-Type-Options header.

Every other case was considered improper implementation (“HTTP”).

50.5% of all tested .gov domains implemented the X-Content-Type-Options header properly. The domain category that properly implemented the X-Content-Type-Options header the best was federal domains by 55%. The domain category that properly implemented the X-Content-Type-Options header the worst was tribal domains by 30%. As a whole, 49.8% of non-federal domains (consisting of city, county, independent intrastate, interstate, state and tribe domains) implemented the X-Content-Type-Options header properly.




![Bar Graph of  X-Content-Type-Options Header Results](/images/govdomains/x-content.png "Bar Graph of  X-Content-Type-Options Header Results")



#### X-Frame-Options (XFO) Header

Websites can use the X-Frame-Options header to express whether or not a browser should be permitted to render the website in a &lt;frame>, &lt;iframe>, &lt;embed> or &lt;object>. This header prevents websites from being embedded in other websites, thus preventing click-jacking attacks (“X-Frame-Options”). These cases were considered improper implementation:



* The website did not implement an XFO header,
* The website implemented XFO, but the header is unrecognizable.

Every other case was considered proper implementation (“HTTP”).

51.9% of all tested .gov domains implemented the XFO header properly. The domain category that properly implemented the XFO header the best was federal domains by 73.9%. The domain category that properly implemented the XFO header the worst was tribal domains by 22.5%. As a whole, 48.7% of non-federal domains (consisting of city, county, independent intrastate, interstate, state, and tribe domains) implemented the XFO header properly.





![Bar Graph of XFO Header Results](/images/govdomains/x-frame.png "Bar Graph of XFO Header Results")



#### The deprecated X-XSS-Protection Header


<table>
  <tr>
   <td><strong>type</strong>
   </td>
   <td><strong>x-xss-protection</strong>
   </td>
  </tr>
  <tr>
   <td><strong>city</strong>
   </td>
   <td>12,4871001
   </td>
  </tr>
  <tr>
   <td><strong>county</strong>
   </td>
   <td>13,89870436
   </td>
  </tr>
  <tr>
   <td><strong>independent intrastate</strong>
   </td>
   <td>14,70588235
   </td>
  </tr>
  <tr>
   <td><strong>interstate</strong>
   </td>
   <td>6,666666667
   </td>
  </tr>
  <tr>
   <td><strong>state</strong>
   </td>
   <td>19,69026549
   </td>
  </tr>
  <tr>
   <td><strong>tribe</strong>
   </td>
   <td>13,33333333
   </td>
  </tr>
  <tr>
   <td><strong>federal</strong>
   </td>
   <td>41,99066874
   </td>
  </tr>
  <tr>
   <td><strong>nonfederal</strong>
   </td>
   <td>13,54330709
   </td>
  </tr>
  <tr>
   <td><strong>all</strong>
   </td>
   <td>17,14
   </td>
  </tr>
</table>


The X-XSS-Protection header was a feature of browsers that stopped pages from loading when they recognized reflected cross-site scripting (XSS) attempts. This header is deprecated and currently only compatible with Safari and Internet Explorer (“X-XSS-Protection”). These cases were considered implementation of the header:



* X-XSS-Protection header set to 1; mode=block, 
* X-XSS-Protection header set to 1.

Every other case was considered to have no implementation (“HTTP”).

17.1% of all tested .gov domains implemented the deprecated X-XSS-Protection header. The domain category that implemented the deprecated X-XSS-Protection header the most was federal domains by 42%. The domain category that implemented the deprecated X-XSS-Protection header the least was interstate domains by 6.7%. As a whole, 13.5% of non-federal domains (consisting of city, county, independent intrastate, interstate, state, and tribe domains) implemented the deprecated X-XSS-Protection header.




![Bar Graph of X-XSS Protection Header Results](/images/govdomains/x-xss.png "Bar Graph of X-XSS Protection Header Results")



### Results by Domain Category




![Bar Graph of Results by Domain Category](/images/govdomains/results_by_type.png "Bar Graph of Results by Domain Category")



#### City

County domains are domains that belong to “Cities, towns, townships, villages, counties, parishes, boroughs, or equivalents” (“Help”). 2907 domains with a .gov TLD and belonging to a city administration were tested. The average .gov domain belonging to a city properly implemented 51.4% of the tested HTTP headers and features. The most properly implemented feature/header was Cross-Origin Resource Sharing (CORS) by 99.8%, meaning nearly all tested city .gov domains implemented proper CORS. The least properly implemented feature/header was HTTP Strict Transport Security (HSTS), by 22.5%. 12.5% of tested city .gov domains implemented the deprecated X-XSS-Protection header.



![Bar Graph of City Results](/images/govdomains/city.png "Bar Graph of City Results")



#### County

County domains are domains that belong to “Cities, towns, townships, villages, counties, parishes, boroughs, or equivalents” (“Help”). 849 domains with a .gov TLD and belonging to a county administration were tested. The average .gov domain belonging to a county properly implemented 50.8% of the tested HTTP headers and features. The most properly implemented feature/header was Cross-Origin Resource Sharing (CORS) by 99.8%, meaning nearly all tested county .gov domains implemented proper CORS. The least properly implemented feature/header was HTTP Strict Transport Security (HSTS) by 31%. 13.9% of tested county .gov domains implemented the deprecated X-XSS-Protection header.




![Bar Graph of County Results](/images/govdomains/county.png "Bar Graph of County Results")



#### Independent Intrastate

Independent Intrastate domains are domains that belong to “An autonomous organization of a single state” (“Help”). 102 domains with a .gov TLD and belonging to an independent intrastate administration were tested. The average .gov domain belonging to an independent intrastate properly implemented 50% of the tested HTTP headers and features. The most properly implemented feature/header was Cross-Origin Resource Sharing (CORS) by 100%, meaning all the tested independent intrastate .gov domains implemented proper CORS. The least properly implemented feature/header was Subresource Integrity by 22.5%. 14.7% of tested independent intrastate .gov domains implemented the deprecated X-XSS-Protection header.



![Bar Graph of Independent Intrastate Results](/images/govdomains/independent-intrastate.png "Bar Graph of Independent Intrastate Results")



#### Interstate

Interstate domains are domains that belong to “An organization of two or more states” (“Help”). 15 domains with a .gov TLD and belonging to an interstate administration were tested. The average .gov domain belonging to an interstate properly implemented 57.8% of the tested HTTP headers and features. The most properly implemented feature/header was Cross-Origin Resource Sharing (CORS) by 100%, meaning all the tested independent intrastate .gov domains implemented proper CORS. The least properly implemented feature/header was Subresource Integrity by 26.7%. 6.7% of tested interstate .gov domains implemented the deprecated X-XSS-Protection header.



![Bar Graph of Interstate Results](/images/govdomains/interstate.png "Bar Graph of Interstate Results")



#### State

State domains are domains that belong to one of “50 U.S. states, District of Columbia (D.C.), American Samoa, Guam, Northern Mariana Islands, Puerto Rico, and U.S. Virgin Islands” (“Help”). 452 domains with a .gov TLD and belonging to a state administration were tested. The average .gov domain belonging to a state properly implemented 49.6% of the tested HTTP headers and features. The most properly implemented feature/header was Cross-Origin Resource Sharing (CORS) by 98.2%. The least properly implemented feature/header was HTTP Strict Transport Security by 27.4%. 19.7% of tested state .gov domains implemented the deprecated X-XSS-Protection header.



![Bar Graph of State Results](/images/govdomains/state.png "Bar Graph of State Results")



#### Tribe

Tribe domains are domains that belong to “Tribal governments recognized by the federal government or a state government” (“Help”). 120 domains with a .gov TLD and belonging to a tribal administration were tested. The average .gov domain belonging to a tribe properly implemented 45.4% of the tested HTTP headers and features. The most properly implemented feature/header was Cross-Origin Resource Sharing (CORS) by 100%, meaning all the tested tribal .gov websites properly implemented CORS. The least properly implemented features/headers were HTTP Strict Transport Security (HSTS) and X-Frame-Options, both by 22.5%. 13.3% of tested tribal .gov domains implemented the deprecated X-XSS-Protection header.



![Bar Graph of Tribe Results](/images/govdomains/tribe.png "Bar Graph of Tribe Results")



#### Non-Federal Domains as a whole

This dataset is a combination of city, county, independent intrastate, interstate, state and tribe domains. 4445 domains with a .gov TLD and belonging to a non-federal administration were tested. The average .gov domain belonging to a non-federal organization properly implemented 50.9% of the tested HTTP headers and features. The most properly implemented feature/header was Cross-Origin Resource Sharing (CORS) by 99.6%. The least properly implemented feature/header was HTTP Strict Transport Security by 25.1%. 13.5% of tested non-federal .gov domains implemented the deprecated X-XSS-Protection header.





![Bar Graph of Results of Non-Federal Domains as a whole](/images/govdomains/nonfederal.png "Bar Graph of Results of Non-Federal Domains as a whole")



#### Federal

Federal domains are domains that belong to “Federal agencies from all three branches of the federal government” (“Help”). 643 domains with a .gov TLD and belonging to the federal administration were tested. The average .gov domain belonging to a federal organization properly implemented 70.8% of the tested HTTP headers and features. The most properly implemented feature/header was Cross-Origin Resource Sharing (CORS) by 99.5%. The least properly implemented feature/header was HTTP Strict Transport Security by 27.2%. 42% of tested federal .gov domains implemented the deprecated X-XSS-Protection header.




![Bar Graph of Federal Results](/images/govdomains/federal.png "Bar Graph of Federal Results")



#### Average

2907 city domains, 849 county domains, 102 independent intrastate domains, 15 interstate domains, 452 state domains, and 120 tribal domains were tested, equaling an amount of 4445 non-federal domains. Additionally, 643 domains belonging to the federal government were tested, equaling 5088 in total. The average of proper header and feature implementation among all tested domains was 53.4%, meaning that, on average, .gov websites implemented around 3 of the 6 tested headers and features. The domain category that implemented most headers and features properly on average was federal domains by 70.8%. The domain category that implemented headers and features properly the least on average was tribal domains by 45.4%.




![Bar Graph of Results on Average](/images/govdomains/site-avg.png "Bar Graph of Results on Average")



## Conclusion/Discussion

**	**
In total, 5088 domains were tested with 7 different tests. The Subresource Integrity (SRI) header was the least properly implemented header type by 32%. Cross-Origin Resource Sharing (CORS) header was the most properly implemented header type by 99.6%. On average, websites with .gov domains belonging to tribal domains implemented HTTP headers and features least properly by 45.4%, and websites with .gov domains belonging to federal domains implemented HTTP headers and features most properly by 70.8%. Additionally, 17.1% of all tested websites implemented the deprecated X-XSS-Protection header. This paper emphasizes that even though they are not the sole indicator of security, each HTTP header has its own important function and is not to be forgotten.



## References


* "Cross-Origin Resource Sharing (CORS)." _MDN Web Docs_, Mozilla, 22 Aug. 2022, developer.mozilla.org/en-US/docs/Web/HTTP/CORS. Accessed 5 Sept. 2022.


* "Help Center - What's an 'authorizing authority', and who is ours?" _DotGov_, Cybersecurity and Infrastructure Security Agency (CISA), home.dotgov.gov/help/#whats-an-authorizing-authority-and-who-is-ours. Accessed 4 Sept. 2022.


* "HSTS." _MDN Web Docs_, Mozilla, 31 July 2022, developer.mozilla.org/en-US/docs/Glossary/HSTS. Accessed 5 Sept. 2022.


* "HTTP Observatory Scoring Methodology." _Mozilla / HTTP Observatory Repository_, GitHub, 4 Sept. 2019, github.com/mozilla/http-observatory/blob/master/httpobs/docs/scoring.md. Accessed 5 Sept. 2022.


* "OWASP Secure Headers Project." _OWASP_, Open Web Application Security Project, owasp.org/www-project-secure-headers/. Accessed 6 Sept. 2022.


* "Subresource Integrity." _MDN Web Docs_, Mozilla, 23 Aug. 2022, developer.mozilla.org/en-US/docs/Web/Security/Subresource_Integrity. Accessed 5 Sept. 2022.


* "X-Content-Type-Options." _MDN Web Docs_, Mozilla, 2 Sept. 2022, developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Content-Type-Options. Accessed 5 Sept. 2022.


* "X-Frame-Options." _MDN Web Docs_, Mozilla, 2 Sept. 2022, developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options. Accessed 5 Sept. 2022.


* "X-XSS-Protection." _MDN Web Docs_, Mozilla, 2 Sept. 2022, developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-XSS-Protection. Accessed 5 Sept. 2022.


## Appendix


### Appendix A

The URL to the GitHub repository hosting all datasets and Python scripts: [https://github.com/erenmenges/Research-Project---Analyze-HTTP-Security-Headers](https://github.com/erenmenges/Research-Project---Analyze-HTTP-Security-Headers)

All issues or pull requests are welcome.
