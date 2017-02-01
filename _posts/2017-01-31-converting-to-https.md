---
layout: post
title: "How to move to HTTPS"
date: 2017-01-31
desc: "How to move to HTTPS"
keywords: "HTTPS"
categories: [Fun]
tags: [Fun]
icon: fa-code
---

# Github stuff

- Create your site repo
- You will want to go to the settings of your repository and set the custom domain.
- Done. :-)


# Cloudflare stuff

- Create free account
- Verify your domain name
- Go to the DNS tab and set 2 A records for your domain name to point to 192.30.252.153 and 192.30.252.154
- You should also get some nameservers you will need to update your domain registrar to point to these 2 names (i.e. jessica.ns.cloudflare.com and kirk.ns.cloudflare.com)
- In the Crypto tab make sure "flexible" is set for SSL.

*Note: once everything is working you can also turn on HSTS in the Crypto section


That should do it. There are plenty of other options available in Cloudflare but I'll leave those for you to discover. :-)