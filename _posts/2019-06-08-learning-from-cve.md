---
layout: post
title: Learning from cve
tag: [fundamental, academic]
---

As a security researcher, it is often necessary to learn from past mistakes. From a broad sight, that is learning from CVE(Common Vulnerabilities and Exposures). CVE entries often provide a basic description and external resources, but to really learn it and understand it, it requires further investigation or even hands-on experiments. In this article, I would describe a scheme I used in an academic assignment (with an example that I used: CVE-2018-0114), for digesting a CVE entry, and hopefully, this scheme would help you.

![theme](/assets/img/2019-06-08-learning-from-cve/theme.png)

## Where are the CVEs

Before you investigate a CVE, you need to know where to find them. [MITRE](http://cve.mitre.org/cve/) is a great resource, click on search you can find any types of cve you want, why not start from [OWASP Top 10](https://www.owasp.org/images/7/72/OWASP_Top_10-2017_%28en%29.pdf.pdf)?

From MITRE you can direct yourself to [National Vulnerability Database (NVD)](https://nvd.nist.gov/vuln/full-listing) wherein most case would also be the description of that vulnerability, and more importantly, analysis, which would include the impact analysis and CVSS(Common Vulnerability Scoring System) scoring.

Another source could be [CVE details](https://www.cvedetails.com/) where you can group vulnerabilities by different scoring, type, etc.

> CVSS Scoring: Evaluating metrics to quantify the risk, is based on a set of values to calculates the potential severity of a vulnerability ([Calculator for CVSS 3.0](https://nvd.nist.gov/vuln-metrics/cvss/v3-calculator)).

## Learn what is THE CVE about

CVE entries are vulnerabilities got patched, they include basic descriptions about the vulnerability, links to the patches, links to PoCs, and any follow up about the vulnerability. [CVE-2018-0114](https://nvd.nist.gov/vuln/detail/CVE-2018-0114).

CVE entries would contain what the vulnerability has an impact on. Generally, a cve lists vulnerable versions of the softwares. Here comes the first step of an investigation, to find out about how widespread this piece of software is. For instance, npm modules have weekly active installations history, which indicates how often would you encounter a possible vulnerable software. 

Apart from the directly impacted software, dependent libraries are also worth investigation.

## Learn the root cause (information to knowledge)

Not all mistakes are unique, people continually making the same mistakes, additional information for digging into would be [CWE (Common Weakness Enumeration)](https://cwe.mitre.org/). It gives you some idea about what leads to this vulnerability.

For instance, [CVE-2018-0114](https://nvd.nist.gov/vuln/detail/CVE-2018-0114) is a JWT issue in early versions of Cisco node-jose library, a mistake letting adversary to forge a fraud token signed by itself. It is not the first of its kind, it is actually a Diffie-Hellman Key Exchange mistake by blindly trust the key in the token header ([CWE322](https://cwe.mitre.org/data/definitions/322.html)). It shows the fact that for a JWT, it is not the JWT itself but rather the key which generates the JWT that has been authenticated. The trust should not be escalated to the whole JWT.

## Learn how to mitigate (information to knowledge)

Mitigation strategy includes applying the patches. However, patches may not always be compatible, or the vulnerable software may be in dependent libraries that do not apply the patch. Sometimes step like sanitization is needed.

For instance, in this node-jose library, when the patch is conflicted with other libraries, it might be better to consider filter out all JWTs including an entire public-key rather than a kid in the header.

Mitigation plans are built based on knowledge and experience, but basic theory, that is to rule out the weakness that causes the issue.
