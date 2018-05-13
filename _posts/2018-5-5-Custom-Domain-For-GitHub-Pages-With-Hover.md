---
layout: post
title: Custom Domain For GitHub Pages With Hover
published: true
categories: GitHub Pages
---

Guide to set up custom domain and subdomain names for your Github Pages. This guide provides examples using [www.hover.com](www.hover.com) as the Internet Domain Name Registration Service. But I believe configuring the other domain name registration service will be the same except the user interface!

## 1. Enable GitHub Pages for your repository

Go to the `settings` page of your repository and enable github pages. Pick a branch to serve your page from.

![github_pages_settings]({{ site.baseurl }}{{ site.imageurl }}/2018-5-5/github_pages_settings.png)

After you have selected the branch and save, github will tell you what is the current url your page is served from at the same settings area.

## 2. Find out the IP address of GitHub DNS

{% highlight bash %}
$ dig taykangsheng.github.io +nostats +nocomments +nocmd
{% endhighlight %}

![github_dns_ip]({{ site.baseurl }}{{ site.imageurl }}/2018-5-5/github_dns_ip.png)

## 3. Configure your Domain Registrar

Point the IP Adressess of GitHub DNS to the root of your domain name. If there are any A record where the host points to root of the domain (hover denotes that using @ as host) or is a wild card (hover denotes that using * as host), remove them and replace them GitHub DNS IP addressess that you have found out above.

![hover_github_dns]({{ site.baseurl }}{{ site.imageurl }}/2018-5-5/hover_github_dns.png)

## 4. Set Custom Domain name at GitHub repository settings page

The front part of the domain is called the subdomain. You can set any subdomain name you want. In this case I wished that this repository is published at **blog.taykangsheng.com**. You can set any subdomain name that you wish.

![github_custom_domain]({{ site.baseurl }}{{ site.imageurl }}/2018-5-5/github_custom_domain.png)

## 5. Match custom domain name at your domain name registrar

Create a **CNAME** record and set HOST value as the subdomain name as specified in part 4, and set the VALUE field to point to your **github_username**.github.io.

![hover_cname]({{ site.baseurl }}{{ site.imageurl }}/2018-5-5/hover_cname.png)

Thats it! Go do something and wait for the changes to propagate throughout the various DNS. Try it after that and you should be able to get to your page using the new custom domain name.

