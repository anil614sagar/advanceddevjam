![](./images/image35.png)

#  Lab 1 – Understanding Flows

## Objectives

In this lab you will get familiar with the Apigee Edge Platform concept of API Proxy Flows. It's one of our developer favorite & most important functionality in Apigee Edge. Apigee Flows feature gives you flexibility to innovate with your API Design, Development & Control API Runtime flow.

In this lab we will see how to create a proxy, Add API resources (Conditional Flows), Add some policies that will be applicable to all the API resources inside the API Proxy, Add some polcies specific to certain endpoints, Reuse policies.

## Prerequisites

- Apigee Edge Account

## Introduction

As you have seen in DevJam Prerequisites labs, You can attach policies to either request or response flows in API Proxy. 

API proxies define request and response flows that are executed in a specific order. The order allows you to apply logic and behavior at specific points in the API proxy execution. The request and response flows are subdivided into proxy and target endpoints (We will learn more about proxy & target endpoints in next lab). Each segment is subdivided into the following flow 'stages':

- **PreFlow**: Always executes before any conditional flows.http://d3grn7b5c5cnw5.cloudfront.net/sites/docs/files/ProxyToBackendWithFlows_v3.png
- **Conditional flows**: One or more flows, each of which has an associated condition. Conditional flows tell Edge, "When you see this, perform this logic." For example, when an API call is a GET on the /movies resource (the condition), transform the response to JSON (through a policy attached to the conditional flow). Only one flow executes per transaction—the first flow whose condition evaluates to true.

	In a conditional flow, the condition is evaluated in both the request and response. You cannot have separate conditions for request and response.

- **PostFlow**: Always executes after any conditional flows. 
- **PostClientFlow**: An optional flow that executes after the response message has been sent to the requesting client app. (This is a specialized flow, and only MessageLogging policies can be attached to it.)

The following figure shows a basic request and response exchange between an app and a backend service. In this figure, the ProxyEndpoint and TargetEndpoint have been expanded to show the flows used to process the request and response: 

  ![Image](images/flows-diagram.png)
  
  This may look complicated, but it's fairly simple once you understand a few use cases.
  
## Part 1. Getting started with a proxy
  
  API Team got a requirement from business team to expose legacy "Customers API" with a new rest design & additional API features like security which will become APP developer friendly and eventually increase the consumption of backend services.
  
### Given Information :
  
  Backend URL : https://apibaas-trial.apigee.net/asagar/macquarie-demo01/customers

  
### Action Items :

 - Create a proxy with a name **{your_initials}\_customers\_proxy\_v1**
 - Base Path should be **/v1/{your_initials}-customers**
 - Deploy to test environment

### Success Criteria :
  
  - API Call to **https://{ORNAME}-test.apigee.net/v1/{your_initials}-customers** should return list of customers.


## Part 2. Introduction to conditionl flows

We now got a new requirement from business team & more details related to legacy customer API from backend team. 

### Given Information :

- Customers API supports GET, POST, PUT, DELETE methods which will allow developers to Retrieve, Create, Update, Delete customers entities. 
- It also has an API to retrieve individual customer details using **https://apibaas-trial.apigee.net/asagar/macquarie-demo01/customers/{customerUUID}** endpoint.

### Business Team Requirements :

- Allow access to 