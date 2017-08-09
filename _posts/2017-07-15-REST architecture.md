---
layout: post
title: REST architecture, HTTP Methods
date: 2017-07-15
tag: NodeJS, web, HTTP
comments: true
---

## What is REST architecture?

REST stands for REpresentational State Transfer. REST is web standards based architecture and uses HTTP Protocol.

REST was first introduced by Roy Fielding in 2000.

A REST Server simply provides access to resources and REST client accesses and modifies the resources using HTTP protocol. Here each resource is identified by URIs/ global IDs. REST uses various representation to represent a resource like text, JSON, XML but JSON is the most popular one.



## most used HTTP Methods

- *GET*

The GET method requests a representation of the specified resource. Requests using GET should only retrieve data and should have no other effect. 
从服务器拿数据 - 查看

- *POST*

The POST method requests that the server accept the data enclosed in the request as a new object/entity of the resource identified by the URI.  
往服务器传数据 - 创建
***Note***: POST is not idempotent, so multiple POST of same object will render multiple new pages.

- *PUT*

The PUT method requests that the server accept the data enclosed in the request as a modification to existing object identified by the URI. If it does not exist then the PUT method should create one.

***Note***: PUT is idempotent, so multiple PUT of same object will substitute previous one.
所以和创建不同，这里是 **更新**


- *DELETE*

The DELETE method requests that the server delete the specified resource.

删除
