# Authentication and Keycloak (Currently Studying)

1) OAuth 2: It was an initiative of several companies that created this open source pattern. It makes it available to someone to access your resource. It is the principal pattern to communicate with a third party platform.
    * Resource Owner: Example: A user (it can be an app/service, too) that wants an app to access his user google drive
    * OAuth Server: The Authorization Server. Example: Keycloak. It says if the user (or app/service) can access the resource or not
    * Client: Example: The client (or app/service) that wants to access the Google Drive
    * Resource Server: Example: The Google Drive

     <img src="https://github.com/fabiolnh/authentication-and-keycloak/blob/main/assets/OAuth%202%20-%20Diagram.png" width="70%">

- Authentication: Example: In a "Event Show", Authentication means that you "show the ingress"
- Authorization: Example: In a "Event Show", Authorization means that where you can go inside into the show (vip, lounge, basic, etc)

2) Open Id Connect (OpenID 2.0): It is the "OAuth 2 + Authentication". It is the "Single Sign-On" pattern (where you can authenticate in one place and access other places). It is the principal Authentication Protocol. It is not the unique, but is the principal.

     <img src="https://github.com/fabiolnh/authentication-and-keycloak/blob/main/assets/Open%20ID%20Connect%20-%20Diagram.png" width="70%">
