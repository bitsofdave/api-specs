## Getting Started with Authentication

In order to use the Namely API, you must first have permission within the system to generate either an API Client or Access Token object.

![](https://files.readme.io/eiT8Yj3zQH2tuq6WxSA9_Screen%20Shot%202014-10-24%20at%202.18.32%20PM.png)

If you don't see the API link on the dropdown, you don't have one of the two required permissions and should contact your company's Namely administrator, or your Namely Client Success Team member to obtain the necessary permissions.

Once you do, you will see a form to create an API client and/or a form to create an Access Token, depending on which permissions you have access to.

In broad overview, an API client allows you to set up an OAuth flow. Using this flow, a user can authorize app access to Namely using permissions. This access is obtained through the use of an Access Token, sent to an app at the end of the OAuth flow. This access token is short lived, with a lifetime of either 15 minutes or 30 days, depending on the exact flow used. This is the preferred way to access the Namely API, as it is significantly more secure.

Creating a Permanent Access Token is a way to bypass the OAuth flow. A Permanent Access Token is significantly longer lived, lasting 2 years. Anyone who has this Access Token is able to access Namely with it alone, which represents a significant security risk. While this access method may be convenient for your purposes (i.e. for a long running server process), we would like to STRONGLY encourage you to take measures to ensure that unauthorized users are not able to obtain your Access Token (e.g. make sure you don't leave it in publicly accessible code repos, etc).

## OAuth: Setting up your API Client

Each API Client has 3 attributes you must set upon creation:

1. Name: a human readable identifier
2. Website: a website for identificaiton
3. Redirect_uri: the URI on YOUR site that successful client authentication will redirect to

---

Once you have successfully created an API Client, you will be provided with its Client Identifier and Client Secret.

For the following examples we will be using this example data:

- Name: 'Hello API World'
- Website: 'http://examplesite.com'
- Redirect URI: 'http://examplesite/api/clients?redirect_success'
- Client Identifier: '696838b8c9fd3f8c8ce21c97313042d2'
- Client Secret: '1e031da1435e8fb5ebeff4ce2278309bf9f1e4f4bc0d676ce187506c250e16bf'

## OAuth: Authorization Code Flow 

The Authorization Code Flow allows for longer term access than the Token Flow using a Refresh Token. A Refresh Token has a lifespan of 30 days, useful if a system will need to make requests on behalf of a user repeatedly over a long period of time.

### Step 1
Direct user to Namely authorization endpoint. This can be done via the main window or a popup window. The client_id and redirect_uri must match the application you created inside of Namely's API section.

```HTTP
GET https://examplesite.namely.com/api/v1/oauth2/authorize?
      response_type=code&client_id=696838b8c9fd3f8c8ce21c97313042d2&redirect_uri=http%3A%2F%examplesite%2Fapi%2Fclients%3Fredirect_success
```

### Step 2
If the user is already logged into Namely, they will be asked to approve the authorization. If they are not currently logged in, they will be prompted to log in and approve the authorization.

### Step 3
Once the user has authorized the request, they will be redirected to the stored redirect uri for the API client used (in this case, http://examplesite/api/clients/redirect_success), with an code parameter containing your Authorization Code.

### Step 4
Send a Post request to the Namely Token Endpoint with your client_identifer, client_secret, and authorization code as part of the request body in URL Encoded format.

```HTTP
POST https://examplesite.namely.com/api/v1/oauth2/token

grant_type=authorization_code&client_id=696838b8c9fd3f8c8ce21c97313042d2&client_secret=1e031da1435e8fb5ebeff4ce2278309bf9f1e4f4bc0d676ce187506c250e16bf&code=[AUTHORIZATION CODE]
```

Step 5: You will receive a JSON response containing an access_token and refresh_token. For more information on using Access Tokens, please consult the "Using an Access Token" section of this documentation. For information on using this Refresh Token to get a new Access Token, read "Using a Refresh Token" below.

## OAuth: Authorization Code Flow, using a Refresh Token

While Access Tokens last for only 15 minutes, a Refresh token lasts for 30 days. During this time it can be used to obtain new Access Tokens, bypassing the full OAuth flow.

### Step 1
Send a Post request to the Namely Token Endpoint with your client_identifer, client_secret, refresh token, and redirect_uri in the request body in URL Encoded format.

```HTTP
POST https://examplesite.namely.com/api/v1/oauth2/token

grant_type=refresh_token&client_id=696838b8c9fd3f8c8ce21c97313042d2&client_secret=1e031da1435e8fb5ebeff4ce2278309bf9f1e4f4bc0d676ce187506c250e16bf&refresh_token=[REFRESH TOKEN]&redirect_uri=http%3A%2F%examplesite%2Fapi%2Fclients%3Fredirect_success
```

### Step 2
At this point you will be redirected to the stored redirect uri for the API client (in this case, http://examplesite/api/clients/redirect_success), with a valid access_token.

## Permanent Access Tokens 

Namely also provides an interface to create long lived Access Tokens. While these can be useful for server processes, we urge you to be cautious. Anyone with access to the Access Token can access your system. Avoid placing such tokens in shared code bases, etc.

The ability to create permanent Access Tokens requires a permission separate from ability to create API Clients, allowing control over which of your uses can do which.

If you decide to use Permanent Access Tokens, the interface is located alongside API Client creation, and will be visible with the proper permission. Please consult your company's Namely or Namely Client Success Representative to have your permissions properly set up.

## Using Access Tokens

In any Namely API request, you need to include your Access Token in your request headers, you should place them in the "Authorization" element in your headers in the format 'Bearer [YOUR ACCESS TOKEN]'.
