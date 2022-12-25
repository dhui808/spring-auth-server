# spring-auth-server
Spring Authorization Server samples

### Build
	mvn clean package

### Why the database is needed for Spring Authorization Server?

## Messages Sample

The messages sample integrates `spring-security-oauth2-client` and `spring-security-oauth2-resource-server` with *Spring Authorization Server*.

The username is `user1` and the password is `password`.

### Run the Message Sample

	Run Authorization Server:
	cd default-authorizationserver
	mvn spring-boot:run

	Run Resource Server:
	cd messages-resource
	mvn spring-boot:run

	Run Client:
	cd messages-client
	mvn spring-boot:run

	Go to http://127.0.0.1:8080


## Federated Identity Sample

The federated identity sample builds on the messages sample above, adding social login and federated identity features to *Spring Authorization Server* using custom configuration.

### 1. Login with Google

1.1 **Set up the project (OAuth Client) in Google API Console**

Follow the instructions on the [Google's OAuth 2.0 implementation](https://developers.google.com/identity/protocols/OpenIDConnect) page, starting in the section, "Setting up OAuth 2.0".

Google's OAuth 2.0 implementation for authentication conforms to the [OpenID Connect 1.0](https://openid.net/connect/) specification and is [OpenID Certified](https://openid.net/certification/).

Complete the "Obtain OAuth 2.0 credentials" instructions

You should have a new OAuth Client with credentials consisting of a Client ID and a Client Secret.

1.2 **Set the redirect URI**

In the "Set a redirect URI" sub-section, ensure that the **Authorized redirect URIs** field is set to `http://localhost:9000/login/oauth2/code/google-idp`.

1.3 **Configure application.yml**

Now that you have a new OAuth Client with Google, you need to configure the application to use the OAuth Client for the *authentication flow*.

Go to `application.yml` and set the following configuration:

```
spring:
  security:
    oauth2:
      client:
        registration:
          google-idp:
            provider: google
            client-id: <your-google-client-id>
            client-secret: <your-google-client-secret>
```

Replace the values in the `client-id` and `client-secret` property with the OAuth 2.0 credentials you created earlier. Alternatively, you can set the following environment variables in the Spring Boot application:
 - `GOOGLE_CLIENT_ID`
 - `GOOGLE_CLIENT_SECRET`

### 2. Login with GitHub

This section shows how to configure Spring Security using Github as an Authentication Provider.

2.1 **Register OAuth application with Github**

To use GitHub's OAuth 2.0 authentication system for login, you must [Register a new OAuth application](https://github.com/settings/applications/new).

2.2 **Set Authorization callback URL (redirect URI)**

Ensure the *Authorization callback URL* is set to `http://localhost:9000/login/oauth2/code/github-idp`.

2.3 **Configure application.yml**

Now that you have a new OAuth application with GitHub, you need to configure the application to use the OAuth application for the *authentication flow*. To do so:

Go to `application.yml` and set the following configuration:

```
spring:
  security:
    oauth2:
      client:
        registration:
          github-idp:
            provider: github
            client-id: <your-github-client-id>
            client-secret: <your-github-client-secret>
```

Replace the values in the `client-id` and `client-secret` property with the OAuth 2.0 credentials you created earlier. Alternatively, you can set the following environment variables in the Spring Boot application:
 - `GITHUB_CLIENT_ID`
 - `GITHUB_CLIENT_SECRET`

### Run the Federated Identity Sample

	Run Authorization Server:
	cd federated-identity-authorizationserver
	mvn spring-boot:run

	Run Resource Server:
	cd messages-resource
	mvn spring-boot:run

	Run Client:
	cd messages-client
	mvn spring-boot:run

	Go to `http://127.0.0.1:8080`

### Note: Spring boot webjars: unable to load javascript library through webjar
	Make sure the path of those js library or css library is correct.
	e.g.:
	
	<link rel="stylesheet" href="/webjars/bootstrap/5.2.3/css/bootstrap.css"/>
	<script src="/webjars/jquery/3.6.1/jquery.min.js"></script>
	<script src="/webjars/bootstrap/5.2.3/js/bootstrap.min.js"></script>

### Resources
[Spring Authorization Server 1.0](https://www.infoq.com/news/2022/12/spring-authorization-server-1-0)       
 	