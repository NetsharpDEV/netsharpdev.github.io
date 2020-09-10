---
layout: post
title: 'Azure Developer: Serverless infrastructure'
date: 2020-09-11
banner_image: az-functions.jpg
tags: [devops, azure, solutions, certificate, serverless]
---

It has been long time since I have written here any post. I was focused on developing myself. In the mean time, I decided to start new series of blog posts,
called Azure Developer. I am learning right now for Azure certification and with these posts I am going to learn and also share what I get from Microsoft documentation.
First of all, I am going to start with serverless infrastructure.
<!--more-->

## Serverless explained

Serverless, buzz word, but what exactly does it mean? It is definitely far from being _without server_. Of course, we have physical server behind scenes but we don't care about management, resource allocation, scalability and configuration. We pay for what we use. That's it - in short.

Microsoft offers variety of different resources we could use. Everything depends on what we really need and what people can manage that.

## Microsoft Power Automate

First, the easiest in management and requiring minimal tech knowledge (how to put couple blocks together) tool we can get from Microsoft. It is built on top of Logic Apps which are built on top of Azure Functions.

This functionality is targeted mainly to people with business background. You create flows having pre-implemented blocks of code with decent description what they actually do. Perfect solution if you don't need any custom implementation and have no time and money for development team. It is also easy to change process and adjust to your need since it is integrated with many popular tools, especially with all Office 365 apps.

I would recommend that tool to every team which has no money or time for developers and need to change flow immediately.

## Azure Logic Apps

Azure Logic Apps are something between Microsoft Power Automate and Azure Functions. It is recommended for developers who want to use pre-implemented solutions and do not want to reinvent the wheel. However, logic apps allow developer to create his own connectors and actions writing a code. They also allow to invoke azure functions as part of flow. Developer can also edit current code of each pre-implemented block of code.

## Azure Functions

In opposite to previous solutions, **functions** are designed as **code-first** services. They are recommended only for developers because of technical knowledge you need to have.

There are several technologies you can use in **Azure Functions**. I don't think that first three will be a suprise to you. It is **C#, F#** and **Powershell Core**. Those are Microsoft technologies and I suppose you expected that as well as I did. But you can also use **Python and Javascript** which is super nice for writing short and easy functions.

From business perspective, Azure Functions should be used whenever you need custom solutions and you have developers to support them. You also need to include time needed for development of each functionality which could be handled by preimplemented solutions in Logic Apps or Power Automate.

## Use-case

I explained to you three options Azure provides us with. But I haven't presented yet how to decide on what I need in specific business case. I will try to describe some situiations and assign potential solution with explanation to it.


# Summary

As you could see, there are many ways of handling serverless compute in Azure. It is not that hard to choose which solution is perfect for your product. You can always combine all of those above, but always ask the questions I tried to answer in this post

* Do I want to change process often and immediately?
* Do I have enough money for development team?
* Are the common connectors and actions solution to my problem?
