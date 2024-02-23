# Authentication and Keycloak (Currently Studying)

1) OAuth 2: It was an initiative of several companies that created this open source pattern. It makes it available to someone to access your resource. It is the principal pattern to communicate with a third party platform.
    * Resource Owner: Example: A user (it can be an app/service, too) that wants an app to access his user google drive
    * OAuth Server: The Authorization Server. Example: Keycloak. It says if the user (or app/service) can access the resource or not
    * Client: Example: The client (or app/service) that wants to access the Google Drive
    * Resource Server: Example: The Google Drive

     <img src="https://github.com/fabiolnh/authentication-and-keycloak/blob/main/assets/OAuth%202%20-%20Diagram.png" width="70%">

- Authentication: Example: In a "Event Show", Authentication means that you "show the ingress"
- Authorization: Example: In a "Event Show", Authorization means that where you can go inside into the show (vip, lounge, basic, etc)

2) OpenID Connect (OpenID 2.0): It is the "OAuth 2 + Authentication". It is the "Single Sign-On'' pattern (where you can authenticate in one place and access other places). It is the principal Authentication Protocol. It is not unique, but is the principal.

     <img src="https://github.com/fabiolnh/authentication-and-keycloak/blob/main/assets/Open%20ID%20Connect%20-%20Diagram.png" width="70%">

- Keycloak: It is not the unique tool for Authentication. There are several. Created by JBoss in 2014. It is open source. It is the main tool used in the market for this purpose. It is very robust to work with a lot of microservices.
  * Some Features: Multiplatform support, Integration with other identity providers (Ex: LDAP with Microsoft Directory, Google, Facebook, etc.), Centralized administration, work with OAuth and OpenID Connect, Login customization, multi-tenancy support, DevOps integration (k8s and docker), There are managed services in clouds, extensible, Open Source.
  * Main Functions:
      - User Authentication (Single Sign-On, with different types of authentications: Login/Password, Multifactor, SSO, etc.)
      - User Authorization (protect access in some resources with permissions and roles)
      - User Management (users, roles, permitions, logs)
      - Integration with different protocols (OAuth 2, OpenID Connect and SAML, to work with different systems)
      - Possibility of customization for each situation/service
      - Session Management (expiration, renew, close)
      - Authentication and Authorization based in levels (configuration of policies in different levels, such as apps, groups, functions, and permission)
