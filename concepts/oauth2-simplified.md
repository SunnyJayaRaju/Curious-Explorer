# 🔑 OAuth 2.0 Simplified

**Date:** Nov 24, 2025 | **Mood:** 🛡️ Secure

## The Problem

Years ago, if a third-party application wanted access to my Google Photos, it often meant giving that application my **Google username and password**.

That's extremely risky.

If the application is compromised, my entire Google account is also at risk.

OAuth 2.0 solves this problem by allowing applications to request **limited permission** without ever seeing or storing my password.

---

## The Analogy: The Hotel Key Card 🏨

I like to think of OAuth 2.0 as checking into a hotel.

1. **The Passport (Authentication)**

   I show my passport to the receptionist once to prove who I am.

2. **The Receptionist (Authorization Server)**

   The receptionist verifies my identity and decides what I'm allowed to access.

3. **The Key Card (Access Token)**

   Instead of giving me the master key to the hotel, the receptionist gives me a key card.

4. **The Permissions (Scopes)**

   My key card opens only:

   - Room 305
   - Gym

   It cannot open:

   - Manager's Office
   - Staff Areas
   - Penthouse Suite

5. **The Expiry**

   After a few days, the key card expires and no longer works.

---

## In the API World

| Hotel | API World |
|--------|-----------|
| Guest | User |
| Receptionist | Authorization Server |
| Hotel Room | Protected Resource |
| Key Card | Access Token |
| Hotel Rules | OAuth Scopes |

The flow looks like this:

1. I sign in to Google.
2. Google verifies my identity.
3. Google issues an **Access Token** to the application.
4. The application sends that token whenever it calls Google's APIs.
5. Google validates the token before allowing access.

At no point does the application know my password.

---

## Key Terms I Learned

### 🔑 Access Token

A temporary credential used to access protected resources.

Think of it as the hotel key card.

---

### 🔄 Refresh Token

A long-lived credential that can be exchanged for a new Access Token after the current one expires.

Think of it as returning to reception to receive a new key card.

---

### 🎯 Scope

Scopes define exactly what an application is allowed to do.

Examples:

- `read:photos`
- `read:profile`
- `write:calendar`

Just because an application has a token doesn't mean it can access everything.

---

### 🚪 Grant Type

A Grant Type defines **how an application obtains an Access Token**.

Common examples include:

- Authorization Code (recommended for user-facing applications)
- Client Credentials (machine-to-machine communication)
- Device Authorization
- Refresh Token

---

## Why Apigee Uses OAuth 2.0

Apigee commonly sits between clients and backend services.

When a request arrives, Apigee can:

- 🔐 Validate Access Tokens.
- 🚫 Reject expired or invalid tokens.
- 🎯 Verify required scopes.
- 🛡️ Protect backend services from unauthorized access.
- 📊 Record authentication and authorization events for monitoring.

Instead of every backend service implementing OAuth validation independently, Apigee centralizes that responsibility.

---

## Authentication vs Authorization

One lesson that finally made OAuth click for me:

- **Authentication answers:** "Who are you?"
- **Authorization answers:** "What are you allowed to do?"

OAuth 2.0 focuses on **Authorization**.

Authentication is often handled by systems such as OpenID Connect (OIDC), which builds on top of OAuth 2.0.

---

## TL;DR

OAuth 2.0 allows users to grant limited access to applications without sharing their passwords.

Instead of giving applications permanent credentials, users grant temporary Access Tokens with specific permissions. API gateways such as Apigee validate these tokens before requests reach backend services, making APIs more secure, scalable, and easier to manage.