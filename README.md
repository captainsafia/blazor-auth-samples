# Authentication in Blazor WASM (.NET 5) with Azure AD and Azure B2C

Blazor in .NET 5 upgrades from MSAL (Microsoft Authentication Library) v1 to v2. This is a major version change so it comes with a few necessary changes. The setup requirement are mostly similar to what was previously required in Blazor WASM 3.2. The notable exception being that redirects can be registered for single-page applications instead of web applications.

Note: While this repository contains code sample, keys/ids/etc and source files containing them have been removed. As a result, the samples here aren't immediately runnable.

MSAL.js v2 supports the Authorization Code Flow with PKCE model for SPA-based clients. This is different from the authentication models that were supported with MSALv1. 

### Azure AD Authneitcaiton

1. Sign in to the Azure portal.
2. Change to the tenant of choice, if needed.
3. Navigate to the "Azure Active Directory" resource.

### Register the server API app
4. Under Manage, select App registrations, then New registration.
5. Add a name for your app and select "Accounts in this organizational directory only" as the authentication type.
6. Change the dropdown in the redirect URI to "Single-page application (SPA)" and add a redirect URI like "https://localhost:5001/login-callback".
7. Select "Expose an API" on the left-hand menu. Then select "Add a scope".
8. You'll be requested to create an application ID. Use the default ID or create your own then continue.
9. In the next form, provide an API scope name and the other requested details.

### Register the client app

10. Go back to the app registerations page as documented in #4.
11. Create a new application and select "Accounts in this organizational directory only" as the authentication type.
11. In the app registerations homeapge, Select the "Add a Redirect URI" link on the right.
12. In the next page, select "Add a platform". In the pop-up, select "Single page application"
13. In the next form, provide a redirect URI and a logout URI then click "Configure".

Now, that we've completed the necessary configurations in the Azure Portal, let's create our Blazor WASM app. Create the application using the following syntax.

```
$ dotnet new blazorwasm -au SingleOrg --api-client-id "{SERVER API APP CLIENT ID}" --app-id-uri "{SERVER API APP ID URI}" --client-id "{CLIENT APP CLIENT ID}" --default-scope "{DEFAULT SCOPE}" --domain "{TENANT DOMAIN}" --tenant-id "{TENANT ID}" -ho -o {APP NAME}
```

### Azure B2C Authentication

1. Sign into the Azure portal.
2. Change to a B2C tenant in the Azure portal. If you don't have a B2C tenant created, you can follow [these instructions](https://docs.microsoft.com/en-us/azure/active-directory-b2c/tutorial-create-tenant) to create one.

### Create a server API app

3. Under Manage, select App registrations, then New registration.
4. Provide a name for the app. Select "Accounts in any identity provider or organizational directory (for authenticating users with user flows)" for the supported account type then click Register.
5. In the app registeration page, select "Expose an API" then select "Add a scope"
6. You'll be prompted to create an application ID URI. Accept the default value and continue.
6. In the next page, provide the scope name and other details requested.

### Create a client app

10. Go back to the app registerations page as documented in #4.
11. Create a new application and select "Accounts in any identity provider or organizational directory (for authenticating users with user flows)" as the authentication type.
11. Select "Single Page Application (SPA)" from the redirect URI dropdown and set the URL to "https://localhost:5001/authentication/login-callback".

### Create a user flow

11. Navigate back to the B2C management portal and select "User flows" in the left-hand column.
11. Select "New user flow" then select "Sign up and sign in" and select "Recommended" for version.
11. Provide a name for the sign up flow and select the identity provider to use.

Now, that we've completed the necessary configurations in the Azure Portal, let's create our Blazor WASM app. Create the application using the following syntax.

```
$ dotnet new blazorwasm -au IndividualB2C --aad-b2c-instance "{AAD B2C INSTANCE}" --api-client-id "{SERVER API APP CLIENT ID}" --app-id-uri "{SERVER API APP ID URI}" --client-id "{CLIENT APP CLIENT ID}" --default-scope "{DEFAULT SCOPE}" --domain "{TENANT DOMAIN}" -ssp "B2C_1_{SIGN UP OR SIGN IN POLICY}" -ho -o {APP NAME}
```
