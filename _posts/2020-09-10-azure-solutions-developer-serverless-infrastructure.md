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

I explained to you three options Azure provides us with. But I haven't presented yet how to decide on what we need in specific business case. I will try to describe some situiations and assign potential solution with explanation to it.

### Ordering system

Let's say we want to create application for processing orders in furniture shop. Main business goal is to fill order request, send it to excel file within company drive and notify specified group of people responsible for orders about new order.

As we can see this is quite simple flow and doesn't require anything custom. Moreover, it may change frequently if company decides to store orders somewhere else than excel. It is possible that in some time they would like to extend or change email notifications to sms.

Next simple question we ask - do we have money or do we need IT team for that? I would say that it is nice to have someone fluent with Microsoft tools, but it doesn't have to be developer since we are not going to implement anything custom.

In this application I would definately recommend to use Microsoft Power Automate. 

However, this is not always that clear. For example, if we have any complicated validation of input or data is processed in some unique way before saving it to database/excel sheet, then we should consider using Azure Functions, or hybrid flow where Logic Apps invoke function as a step in the process.

For more complicated operations, time consuming there are Durable Functions. I will write about them in the other articles. 


# Summary

As you could see, there are many ways of handling serverless compute in Azure. It is not that hard to choose which solution is perfect for your product. You can always combine all of those above, but always ask the questions I tried to answer in this post

* Do I want to change process often and immediately?
* Do I have enough money for development team?
* Are the common connectors and actions solution to my problem?
