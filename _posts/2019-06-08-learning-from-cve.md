---
layout: post
title: Learning from cve
tag: [fundamental, academic]
---

As a security researcher, it is often necessary to learn from the past mistakes. From a broad sight, that is learn from CVE(Common Vulnerabilities and Exposures). CVE entries often provide basic description and external resources, but to really learn it and understand it, it requires further investigation or even hands on experiments. In this article, I would describe a scheme I used in academic assignment (with an example that I used : CVE-2018-0114), for digesting a CVE entry, and hopefully this scheme would help you.

![theme](/assets/img/2019-06-08-learning-from-cve/theme.png)

## Where are the CVEs

Before you investigate a CVE, you need to know where to find them. [MITRE](http://cve.mitre.org/cve/) is a great resource, click on search you can find any types of cve you want, why not start from [OWASP Top 10](https://www.owasp.org/images/7/72/OWASP_Top_10-2017_%28en%29.pdf.pdf)?

From MITRE you can direct yourself to [National Vulnerability Database (NVD)](https://nvd.nist.gov/vuln/full-listing) where in most case would also be the description of that vulnerability, and more importantly, analysis, which would include the impact analysis and CVSS(Common Vulnerability Scoring System) scoring.

Another source could be [CVE details](https://www.cvedetails.com/) where you can group vulnerabilities by different scoring, type, etc.

> CVSS Scoring: Evaluating metrics to quantify the risk, it based on a set of values to calculates the potential severity of a vulnerability ([Calculator for CVSS 3.0](https://nvd.nist.gov/vuln-metrics/cvss/v3-calculator)).

## Learn what is THE CVE about

CVE entries are vulnerabilities got patched, they include basic descriptions about the vulnerability, links to the patches, links to PoCs, and any follow up about the vulnrability. [CVE-2018-0114](https://nvd.nist.gov/vuln/detail/CVE-2018-0114).

CVE entries normally would contain what the vulnerability has impact on. In general it would give out the versions of software that is vulnerable to it as it is directly being affected. Here comes the first stage of investigation, to find out about how popular this piece of software is, for instance, npm modules normally have active installations history per week, which would be a great indicator of how often would you encounter a possible vulnerable software. Besides the direct affected softwares, dependent libraries are also worth investigating.

## Learn the root cause (information to knowledge)

Not all mistakes are unique, people constantly making the same mistakes, additional information for digging into would be [CWE (Common Weakness Enumeration)](https://cwe.mitre.org/). It gives you some idea about what leads to this vulnerability.

For instance, [CVE-2018-0114](https://nvd.nist.gov/vuln/detail/CVE-2018-0114) is a JWT issue in early versions of Cisco node-jose library, a mistake letting adversary to forge a fraud token signed by itself. It is not the first of its kind and deep in the core, it is actually a Diffie-Hellman Key Exchange mistake by blindly trust the key in the token header as authenticated ([CWE322](https://cwe.mitre.org/data/definitions/322.html)). It shows the fact that for a JWT, it is not the JWT itself that has been authenticated, but rather the key which generates the JWT that has been authenticated. The trust should not be escalated to the whole JWT.

## Learn how to mitigate (information to knowledge)

Mitigation strategy normally including applying the patches. However, patches may not always be compatiable, or the vulnerablee software may be in dependent libraries that do not apply the patch. Sometimes steps like sanitizations are needed.

For instance, in this node-jose library, when patch is conflicted with other libraries, it might be better to consider a sanitization step to filter out all JWTs including an entire public-key rather than kid in the header, which bypass this vulnerability.

Mitigation plans are built based on knowledge comes with experience, but the main idea is simple, that is to rule out the weakness caused the issue.
