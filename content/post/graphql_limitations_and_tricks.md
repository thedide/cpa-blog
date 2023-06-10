---
title: "Graphql Limitations and Tricks"
date: 2023-06-09T17:48:49-07:00
draft: true
categories:
  - Software Development
  - Technology
tags:
  - GraphQL
  - Searchable
  - AWS AppSync
---

# Graphql Limitations and Tricks

## Introduction:
GraphQL has emerged as a popular choice for building APIs due to its flexible and efficient approach to data fetching and manipulation. However, like any technology, GraphQL has its limitations. In this blog post, we'll explore some common GraphQL limitations and discuss effective strategies and workarounds to overcome them. By understanding these limitations and employing best practices, developers can harness the full potential of GraphQL in their applications.

## Indexing

## Case insensitive query

## Filter based on



## Request failed with status code 401

In case you get "ERROR {"data": {}, "errors": [[GraphQLError: Request failed with status code 401]]}" your api key should be expired, you can go to AWS AppSync console (https://aws.amazon.com/appsync/) to create a new one and you will need to update that key on your aws-exports file/amplify config as well located at "/PathToProjectRoot/aws-exports.js" and a couple of amplify-meta.json files (search for old key in your project to find the files to be modified).

If the issue still persists clean the project by running:
```
cd android
./gradlew clean
```

If this does not help either, run the following:
```
watchman watch-del-all watchman shutdown-server
```
