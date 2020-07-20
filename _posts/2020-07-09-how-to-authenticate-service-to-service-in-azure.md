---
layout: post
title: 'How to authenticate service-to-service in Azure Active Directory'
date: 2020-07-09
banner_image: az-functions.jpg
tags: [development, azure, solutions, distributed services]
---

There are couple of existing ways how to authenticate one app to another when you create distributed system. You can write your own identity service and use it across your apps.  
But here Azure Active Directory comes with its great features I personally love. In this post I will show you how to configure one of those flows.
<!--more-->

## User wants to fetch its ID data

Let's assume we have a gateway service called Gateway.API and user service called User.API which preserves user data. We assume that nobody but user itself can access its data. But how User.API will know that the Gatway.API is requesting it in the name of the User? Here [**OAuth2.0 On Behalf Authentication flow**](https://docs.microsoft.com/en-us/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow) comes to the rescue. 

The main idea is to propagate user identity and its permissions to another element in a chain - to service which is requested by service requested by user. In this way we can easily configure access to specific resources thanks to AD.

### OAuth 2.0

What OAuth 2.0 is and how exactly it works you can read on its [official website](https://auth0.com/docs/protocols/oauth2).
But let me give you a short introduction. Basic idea is that client (application requesting access) asks Resource Owner to 

![on_behalf](/images/posts/onBehalf.png)

### How to configure Azure Active Directory?

Configuration of Azure Active Directory is quite simple. Basically, Azure AD already exists on our Azure account. To register a new app which will be using it we have to go to "App Registrations" and click ".New registration".
Then you should see the following screen.

![ad_registration](/images/posts/ad-registration.png)

In the **Name** field we might type whatever we want to correctly identify the app.
More important are next two sections.
- Supported account types - tells the AD if we want to allow to login only users from our tenant (company) or also grant access to other tenants / personal accounts. I want my app to be used only in my company so I will go for the first option. Meaning, only people with users created for this tenant will be able to login. They don't have to be in the same domain name like **@netsharpdev.com**. Users you can define in section called **Users** within Azure Active Directory section.
- Redirect URI - this is optional option, but if we want to authenticate with impersonate flow, we need to setup a redirect url. It is an url of our app. The impersonation flow is the way of authenticating when user clicks login, a popup is being shown where he types his microsoft credentials. Redirect url says where we want to come back after authentication. In my case application is hosted locally so the redirect URL will be **https://localhost:8080**. You can define more than one redirect url if you are going to use the same registration in different places. Most probably scenario is when you want to connect your local app to test or production app registration to easily use the same setup. To add more than one url you must create app registration and then go to its settings.

![auth_app_reg](/images/posts/auth-app-reg.png)

However, I want to focus on **On Behalf Authentication Flow** in this post, we might erase *http://localhost:8080* url from configuration.

#### Configure access to app

I have created one more app registration called **test-client**. It represents the client app which is meant to authenticate to **test** on behalf of authorized user.

If you want to grant **test-client** access to **test** you have to do two steps. First of all, in **test** app registration you must create scope. You can have multiple scopes and decide on code level which scope gives access to which part of the app. I am going to create **user_impersonation** scope. According to documentation it is default one meaning we are going to impersonate a user who is already authorized in client app. 

Below you can see the view from Azure portal when I was creating scope for my app.

![create-scope](/images/posts/create-scope.png)

In the second step, we must grant our client app access to specific scopes.

![add-client](/images/posts/add-client.png)



### How to implement that in .NET Core 3.1?

Currently, there is a library under development and still in prelease version written by Microsoft community to make it easier, but creating production apps we want to have stable implementation. However, I will use some of the extensions provided by this preview to avoid implementing complex solutions when the problem has already been solved. Stable version is expected to be released by the end of the year 2020.

Let start with proper setup in appsettings.json. Here, we need to provide Azure AD **instance** which is by default "https://login.microsoftonline.com/",
**domain** and **tenantId**, can be found in Active Directory overview and **clientId** which is the id of our app registration. It binds our app with specific app registration.


#### appsettings.json
```json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "Domain": "netsharpdev.com",
    "TenantId": "80c55bf4-64c0-4b58-965e-f2398ba61766",
    "ClientId": "642b4ac0-ccc4-4e02-a626-47c24531820e"
  },
```
<!-- change issuer -->

<!-- say more about used nugets -->
#### Startup.cs
```csharp
var azureAdConfig = Configuration.GetSection("AzureAd")
                .Get<AzureAdConfiguration>();r
```



```csharp
   services.AddAuthentication(AzureADDefaults.JwtBearerAuthenticationScheme)
        .AddAzureADBearer(
            options =>
                {
                    options.Domain = azureAdConfig.Domain;
                    options.Instance = azureAdConfig.Instance;
                    options.ClientId = azureAdConfig.ClientId;
                    options.TenantId = azureAdConfig.TenantId;
                    })
```

<!-- Write some code here -->

```csharp
            services.Configure<JwtBearerOptions>(AzureADDefaults.JwtBearerAuthenticationScheme, options =>
            {
                options.Authority += "/v2.0";
                options.TokenValidationParameters.ValidAudiences = new[]
                {
                    options.Audience
                };
            });

```