# Drupal 11 + Azure AD OpenID Connect Integration
Objective: Enable users to log in to a Drupal 11 site using their Microsoft Entra ID (Azure AD) accounts via OpenID Connect (OIDC).

## Create an App Registration in Azure AD

  1. Go to Azure Portal → Entra ID → App registrations → New registration.
  
  2. Fill in:
  
    Name: Drupal OIDC Login
    
    Supported account types: Usually “Accounts in this organizational directory only”
    
    Redirect URI (Web): Will use Drupal’s callback URL 
    
    Click Register.
    
    <img width="777" height="645" alt="image" src="https://github.com/user-attachments/assets/9b2d4aef-67c5-4648-9227-10b39df8d35d" />


## Create Client Secret

  1. In your app registration, go to Certificates & secrets → New client secret.
  
  2. Add a description (e.g., Drupal secret) and expiration.
  
  3. Click Add.
  
  4. Copy the secret value — this will be used in Drupal configuration.

## Configure Redirect URI in Azure

  In Authentication → Redirect URIs, add your Drupal callback URL:
  ```
  https://kahramaa.dev/openid-connect/drupal_oidc_login
  ```

  Ensure ID tokens are checked (required for OIDC login).

  Save changes.

⚠️ The redirect URI must exactly match what you enter in Drupal.

## Configure Drupal OpenID Connect Module

  Go to Drupal Admin → Configuration → People → OpenID Connect → Clients → Add client → Generic, Fill in the fields:
  
    Eg:
    Name	Drupal OIDC Login
    Client ID	hjsjsjsjj
    Client Secret	jhssjshsh
    Authorization Endpoint	https://login.microsoftonline.com/something
    Token Endpoint	https://login.microsoftonline.com/something
    UserInfo Endpoint	https://graph.microsoft.com/oidc/userinfo
    Scopes	openid profile email
    Redirect URL / Callback URL	https://kahramaa.dev/openid-connect/drupal_oidc_login

  Save the configuration.

  Open a private/incognito browser window. Visit your Drupal login page → Click Login with Microsoft. Authenticate using your Azure AD credentials. Upon success, Drupal should create a user or link to an existing user using the email returned from Azure.
  
