---
layout: post
title: Example GraphQL Request
published: true
categories: graphql ruby
---

```ruby
require 'http' # https://github.com/httprb/http
def JSON_pretty_print(string); JSON.pretty_generate(JSON.parse(string)) end # function to pretty print json string

## Example 1
# GraphQL Swapi Server hosted on local computer at http://localhost:58893
# https://github.com/graphql/swapi-graphql
body = HTTP.post(
    "http://localhost:58893",
    json: {
        query: <<~GRAPHQL
            query {
                __schema {
                    queryType {
                        name
                    }
                }
            }
        GRAPHQL
    }
).body

puts JSON_pretty_print(body)
# {
#   "data": {
#     "__schema": {
#       "queryType": {
#         "name": "Root"
#       }
#     }
#   }
# }

## Example 2
# GitHub's GraphQL endpoint
git_hub_response = HTTP.auth("Bearer myfakegithubaccesstoken").post(
    "https://api.github.com/graphql",
    json: {
        query: <<~GRAPHQL
            query {
                viewer {
                    name

                }
            }
        GRAPHQL
    }
).body

puts JSON_pretty_print(git_hub_response)
# {
#   "data": {
#     "viewer": {
#       "name": "Tay Kang Sheng"
#     }
#   }
# }
```