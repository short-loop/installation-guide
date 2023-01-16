---
title: GitHub SSO Setup
layout: default
filename: github-sso.md
--- 


# ShortLoop GitHub Single Sign-On Setup

ShortLoop can use GitHub identities to log into your deployed instance. Follow the steps to enable it.

1. **Navigate to the [Organizations section](https://github.com/settings/organizations) of your profile and the settings of the organization under which you want to create an OAuth App.**
    
   ![Github Organization section](/images/sso/1.png)

2. **Navigate to the OAuth Apps section of the organization settings page.**
   
   ![Github org OAuth Apps settings](/images/sso/2.png)

3. **Create a new OAuth App.**
   
   ![Github create a new OAuth app](/images/sso/3.png)

4. **Configure the OAuth Credentials for your ShortLoop deployment.**
   
   Homepage URL: IP or domain on which your ShortLoop instance is deployed. (ex: https://shortloop.acmeinc.org/)

   Authorization callback URL: Append /login/oauth2/code/github to the Homepage URL (ex: https://shortloop.acmeinc.org/login/oauth2/code/github)

   ![Github enter OAuth app details](/images/sso/4.png)

5. **Copy the Client ID and store it somewhere.**
   
   ![Github copy OAuth client ID](/images/sso/5.png)

6. **Generate a new client secret**
   
   ![Github generate a new client secret](/images/sso/6.png)

7. **Copy the client secret and store it somewhere safe. Do not share this in public.**
   
   ![Github copy OAuth client secret](/images/sso/7.png)

8. **Go back to ShortLoop Setup SSO screen and click on "Get Started".**

   ![Github copy OAuth client secret](/images/sso/8.png)

9. **Enter the copied client ID and client secret for your GitHub OAuth app.**
   
   > NOTE: We do not verify Oauth2 client ID and secret. Please make sure you copy the correct values before enabling it in your ShortLoop instance. Entering the wrong credentials can result in no one being able to log in. To fix it, you'll have to delete GitHub's OAuth credentials from your instance's database.

   ![Github copy OAuth client secret](/images/sso/9.png)

Once SSO gets enabled, you'll get a success alert.
