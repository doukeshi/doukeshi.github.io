---
layout: post
title: 图解 OIDC/OAuth2 授权认证流程
tags:
- Illustrate
- OAuth2
- OIDC
- Authentication
- Authorization
---

## Authorization Code Flow

![auth code flow]({{ "images/2022-07/oauth2-auth-code.svg" | relative_url }})

### Terminology

**State**

Opaque value used to maintain state between the request and the callback. Typically, Cross-Site Request Forgery (CSRF, XSRF) mitigation is done by cryptographically binding the value of this parameter with a browser cookie.

**Claim:**

Piece of information asserted about an Entity.

**ID Token:**

JSON Web Token (JWT) that contains Claims about the Authentication event. It MAY contain other Claims.

**End-User(EU):**

> **Resource owner** in OAuth2

Human participant.

**OpenID Provider(OP):**

> **Authorization server** in OAuth2
>
> **Resource server** in OAuth2.
> The resources in this case is just what [UserInfo Endpoint] return.

OAuth 2.0 Authorization Server that is capable of Authenticating the End-User and providing Claims to a Relying Party about the Authentication event and the End-User.

**Relying Party(RP):**

> **Client** in OAuth2

OAuth 2.0 Client application requiring End-User Authentication and Claims from an OpenID Provider.

前后端分离场景下，RP 可细分为 User Agent(UA, 客户端) 和 Backend Service(RP, 服务端)，如图。

### Endpoint:

**Authorization Endpoint:** The Authorization Endpoint performs Authentication of the End-User.

**Token Endpoint:** To obtain an Access Token, an ID Token, and optionally a Refresh Token, the RP (Client) sends a Token Request to the Token Endpoint to obtain a Token Response when using the Authorization Code Flow.

**UserInfo Endpoint:** The UserInfo Endpoint is an OAuth 2.0 Protected Resource that returns Claims about the authenticated End-User. The UserInfo Endpoint MUST accept Access Tokens as OAuth 2.0 Bearer Token.

<br>

## PKCE(Proof Key for Code Exchange)

![auth code with pkce flow]({{ "images/2022-07/oauth2-auth-code-pkce.svg" | relative_url }})

### Terminology

**code verifier** > x

A cryptographically random string that is used to correlate the authorization request to the token request.

**code challenge** > y

A challenge derived from the code verifier that is sent in the authorization request, to be verified against later.

**code challenge method** > y = f(x)

A method that was used to derive code challenge.

- plain: code_challenge == code_verifier
- S256: code_challenge = BASE64URL-ENCODE(SHA256(ASCII(code_verifier)))

<hr>

REF:
- https://openid.net/specs/openid-connect-core-1_0.html
- https://datatracker.ietf.org/doc/html/rfc7636
