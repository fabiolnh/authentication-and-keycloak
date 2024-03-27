 # Authentication and Keycloak (Currently Studying)

1) OAuth 2: It an Authorization protocol. It was an initiative of several companies that created this open source pattern. It makes it available to someone to access your resource. It is the principal pattern to communicate with a third party platform.
    * Resource Owner: Example: A user (it can be an app/service, too) that wants an app to access his user google drive
    * OAuth Server: The Authorization Server. Example: Keycloak. It says if the user (or app/service) can access the resource or not
    * Client: Example: The client (or app/service) that wants to access the Google Drive
    * Resource Server: Example: The Google Drive

     <img src="https://github.com/fabiolnh/authentication-and-keycloak/blob/main/assets/OAuth%202%20-%20Diagram.png" width="70%">

OBS: 
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
      - Email verification
   * Important Things:
      - If you have a group of users in another identity provider, you have to migrate it to keycloak or use IDs to refer.
      - Roles (what the user can do in the keycloak) can have other roles
      - Realm = "Reino''. So, It is a multi-tenancy support. It is to segment the keycloak (by companies, per example).
      - Each Realm works with an independent environment, isolating completely the identity data (users, groups, roles, etc.) from other realms in the same Keycloak Server.
      - Group: A group of Roles
      - You can define one internalization for each Realm (different languages for each Realm)
      - Client: The application that accesses the resource from Resource Server will be the "Client". So, when the Application needs to use the Identity Provider (Keycloak), this application will be the "Client".

   - The tokens are formatted as JWT (Header, Payload and Signature). Do not put sensible information in JWT because it can be decoded.
   - Access Token: (OAuth2) Authorization Token to access the Application. To access the keybloak to consume it or the applications of the ecosystem.
   - ID Token: (OpenID Connect) Authentication Token. To get it, use the "scope" parameter (with value "openid") in the REST request. It will return the id_token in the JWT format, too.

   - Authentication Streams ("Fluxos de Autenticação"): (OBS: Involves Authentication and Authorization)
      1) Authorization Code: Commonly used in Web Applications. Redirects the user to login in the keycloak environment and then, keycloak redirects to the application and the process is ended. Good for mobile, too.
      <img src="https://github.com/fabiolnh/authentication-and-keycloak/blob/main/assets/Authorization%20Code%20-%20Fluxo.png" width="100%">
         Implementation:
         
          - In the Frontend of the App: create a GET to "KeycloakAddress"/realms/.../protocol/openid-connect/auth (with query params: client_id, redirect_url:"backend endpoint url", response_type:'code', scope:'openid'). It will redirect to login in the Keycloak.
          - In the Backend (in the Callback Endpoint): create a POST to "KeycloakAddress"/realms/.../protocol/openid-connect/token" (with params: client_id, grant_type:'authorization_code', code:"code from response", redirect_uri:"frontend url for callback"

         Authorization Code Attack:

          - Replay Attack: In the Network, the attacker captures the callback url with the same code, and does a replay of the call back url concurrently. The keycloak returns two valid tokens. How to be protected? With "Nonce" (Number Used Once). To implement it, it has to be created an aleatory value and kept in the user session. With this number, send it to keycloak in the body of "/auth" (url: /login in front). Then, verify if the nonce sent is equal to the noncy received in the token. This strategy works when we do not have a Client totally public. (Because the code is in the javascript of the front end). Several libs already implement it.
          - CSRF Attack: (Cross Site Request Forgery) An attack that forces the user to execute a call using the values in the session/cookies of the website to do other actions/intentions (in another tab of the browser). How to be protected? With another code ("state" param). It has to be sent, too, through "/auth" in the body. This "state", when it returns to the backend, it returns in the url, not inside the token (differently from "nonce"). Verify if it is equal. Why the attacker cannot do the attack with this implementation? The attacker will not have access to the state. Even if he figure out the state, the nonce will not be equal (so, if you implement both, you will be protected)

      2) Implicit Flow: Simplify the process of obtaining a token. Only the frontend does the process (SPA). Differently from "Authorization Code", with this process, we already received the token in the url (without the exchanging of the "code"). Put the token in the cookie or wherever you want. If you think, the token in the url involves risks. It is used only for an intern environment, where we have trustability. Also, URLs usually are recorded historically or by monitoring in the network (another risk). This process is faster than "Authorization Code", but it is too risky.
      <img src="https://github.com/fabiolnh/authentication-and-keycloak/blob/main/assets/Implicit%20Flow.png" width="70%">

      3) Hybrid Flow: Similar to Implicit Flow. It is the union of Implicit Flow + Authorization Code. The intention is to work with the front end app, too. The front end receives the Authorization Code and the Token. There is more security and performance. You can use single sign on, too.
      <img src="https://github.com/fabiolnh/authentication-and-keycloak/blob/main/assets/Hybrid%20Flow.png" width="70%">

      4) Resource Owner Password Credential (or Direct Grant): The user accesses the application, but the application will not redirect the user to keycloak. The Application has its own login form. after login, there is a http request to the identity server, then return the token to the client application. The application manipulates the credentials. There are some security risks on it. It is recommended only for trustable environments and the need for a simple authentication. You lose some resources, such as: google login, facebook login, two factor, etc. 
      <img src="https://github.com/fabiolnh/authentication-and-keycloak/blob/main/assets/Resource%20Owner%20Password%20Credentials.png" width="70%">
