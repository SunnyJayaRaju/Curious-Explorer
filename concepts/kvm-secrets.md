# 🔐 Key Value Maps (KVMs): Keeping Configuration and Secrets Secure

**Date:** Nov 27, 2025 | **Mood:** 🤫 Secure

## The Problem

Imagine my API needs to connect to a backend service.

That backend requires:

- API Keys
- Passwords
- Endpoint URLs
- Timeouts
- Feature flags

One option would be to hardcode these values directly into the API proxy.

```xml
<Password>Hunter2</Password>
```

That's a terrible idea.

If the value changes, I have to redeploy the proxy.

If it's a password, anyone with access to the source code can potentially see it.

Configuration should be separated from code.

That's where **Key Value Maps (KVMs)** help.

---

## The Analogy: The Safe Deposit Box 🏦

I think of a KVM as a collection of safe deposit boxes inside a bank vault.

The vault is the KVM.

Each box has:

- A key (the entry name)
- A stored value

Examples:

| Key | Value |
|-----|-------|
| backend_url | https://api.company.com |
| timeout | 5000 |
| db_password | ******** |

When my API proxy runs, it opens the correct box, retrieves the value, and uses it while processing the request.

The configuration stays outside the proxy code.

---

## Why Use KVMs?

KVMs make API proxies easier to maintain.

Instead of changing XML policies whenever configuration changes, I can update the KVM.

Typical use cases include:

- Backend URLs
- API Keys
- Timeouts
- Feature flags
- Environment-specific configuration
- Credentials (when appropriate)

---

## Encrypted vs Unencrypted Entries

### Unencrypted

Suitable for ordinary configuration values such as:

- Backend URLs
- Timeout values
- Feature flags

These values are not considered sensitive.

---

### Encrypted

Suitable for sensitive values such as:

- Passwords
- API Keys
- Client Secrets

Encrypted KVM entries are protected by Apigee and are intended for use by runtime policies.

> **Important:** For highly sensitive production secrets, many organizations integrate Apigee with dedicated secret management solutions such as Google Cloud Secret Manager or HashiCorp Vault. KVMs are excellent for configuration, but they are not a full enterprise secret-management platform.

---

## Example Policy

A KVM value can be retrieved using the `KeyValueMapOperations` policy.

```xml
<KeyValueMapOperations mapIdentifier="SecretsVault" name="Get-Password">
    <Get assignTo="private.backend_password">
        <Key>
            <Parameter>db_pass</Parameter>
        </Key>
    </Get>
</KeyValueMapOperations>
```

Notice the variable name begins with:

```text
private.
```

This prevents the value from being displayed in the Apigee Debug session, helping reduce accidental exposure of sensitive information.

---

## My Rule of Thumb

I try to follow one simple principle:

> **Code defines behaviour. KVMs provide configuration.**

That separation makes APIs easier to maintain, deploy, and secure.

---

## TL;DR

A Key Value Map (KVM) stores configuration separately from API proxy code.

It allows Apigee policies to retrieve values such as URLs, API Keys, timeouts, and passwords at runtime without hardcoding them into the proxy. This improves maintainability and supports more secure configuration management.

---

## 🔗 Continue Exploring

- ⬅️ Back to the [Repository Home](../README.md)