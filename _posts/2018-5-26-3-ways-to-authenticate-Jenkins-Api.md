---
layout: post
title: 3 ways to authenticate Jenkins Api
published: true
categories: jenkins ruby
---

### 1. Username and Password

```ruby
require 'http'

jenkins_server = 'jenkins.example.com'
jenkins_usr = 'ajenkinsuser@example.com'
jenkins_pwd = 'averysecurepassword123!'

response = HTTP
  .get("https://#{jenkins_usr}:#{jenkins_pwd}@#{jenkins_server}/job/rubygems/job/mygem/job/master/api/json")
```

**Pros:** Minimal Setup  
**Cons:** Generally its not a good idea for password to be out in clear text, especially for apps working in production.

### 2.Username and Api Token

You can obtain your Api Token in your Jenkins user profile page. 

```ruby
require 'http'

jenkins_server = 'jenkins.example.com'
jenkins_usr = 'ajenkinsuser@example.com'
jenkins_api_token = 'ajenkinsapitoken123!'

response = HTTP
  .get("https://#{jenkins_usr}:#{jenkins_api_token}@#{jenkins_server}/job/rubygems/job/mygem/job/master/api/json")
```

**Pros:** Minimal Setup, Api Token can be revoked easily.  
**Cons:** Long URL, It is also not a really good idea for the api token to be out in clear text, especially for apps working in production.   

### 3.Basic auth token

Jenkins provide a way to authenticate its api using `Basic` auth and a `base64` hash of the username and api token. 

```ruby
jenkins_server = 'jenkins.example.com'
jenkins_usr = 'ajenkinsuser@example.com'
jenkins_api_token = 'ajenkinsapitoken123!'

auth_token = Base64.strict_encode64("#{jenkins_usr}:#{jenkins_api_token}")

response = HTTP
  .auth("Basic #{auth_token}")
  .get("https://jenkins.in.spdigital.io/job/ArchitectureProcesses/job/dashii/job/master/api/json")
```

**Pros:** You can hash your auth token beforehand and your jenkins username and api token doesn't need to be in clear text. Short URL!  
**Cons:** Someone can simply `base64` decode your hash.. but oh well as long as its not in clear text. 🤤
