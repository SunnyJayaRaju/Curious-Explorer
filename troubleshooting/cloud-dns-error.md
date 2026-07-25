# 🐛 Troubleshooting: "This Site Can't Be Reached" After Deployment

**Date:** Dec 1, 2025 | **Context:** First Apigee X Cloud Deployment

## The Problem

I successfully deployed my API proxy to Apigee.

Deployment completed successfully.

Everything showed a green status.

But when I opened the API URL in my browser, I saw:

```
ERR_NAME_NOT_RESOLVED

This site can't be reached.
Server IP address could not be found.
```

At first, I assumed something had gone wrong with my proxy deployment.

---

## What Does This Error Mean?

`ERR_NAME_NOT_RESOLVED` is a DNS (Domain Name System) error.

DNS is responsible for translating a hostname (such as `api.example.com`) into an IP address that computers can reach.

If DNS cannot find the hostname, the browser doesn't even know where to send the request.

The request never reaches Apigee.

---

## Investigation

Rather than immediately changing configuration, I checked each layer one by one.

### ✅ Step 1 — Verify Deployment

The proxy showed:

```
Deployment Status: Deployed
```

So the proxy itself wasn't the problem.

---

### ✅ Step 2 — Verify Internet Connectivity

Other websites loaded correctly.

My local network was working.

---

### ✅ Step 3 — Check Environment Group Hostnames

Inside Apigee:

```
Admin
 └── Environments
      └── Environment Groups
```

I inspected the configured hostnames.

This turned out to be the key clue.

---

## Root Cause 🔍

I assumed my organization would use the default:

```
*.apigee.net
```

hostname.

However, my Evaluation Organization had not configured DNS for that hostname.

Instead, Google had assigned a hostname based on **`nip.io`**, a wildcard DNS service that automatically maps hostnames containing an IP address to that IP.

For example:

❌ Incorrect

```
resonant-amulet...apigee.net
```

DNS lookup failed.

✅ Correct

```
35.186.244.11.nip.io
```

This hostname correctly resolved to the Apigee load balancer.

The proxy had been working all along—I was simply using the wrong hostname.

---

## The Fix

I updated my requests to use the hostname listed under the Environment Group configuration.

As soon as I switched to the correct `nip.io` address, the API responded successfully.

No proxy changes were required.

---

## Troubleshooting Checklist

Whenever I see a DNS-related error after deployment, I now check:

- ✅ Was the proxy deployed successfully?
- ✅ Is my internet connection working?
- ✅ Am I using the correct Environment Group hostname?
- ✅ Does the hostname resolve to an IP address?
- ✅ Is DNS configured for this environment?

Only after those checks would I investigate the proxy itself.

---

## Lesson Learned

One lesson really stood out to me:

> **A successful deployment doesn't guarantee a reachable API.**

Deployment verifies that Apigee accepted the proxy.

Connectivity depends on infrastructure such as DNS, hostnames, and load balancers.

Always verify the network path before debugging application logic.

---

## TL;DR

My API proxy deployed successfully, but the browser returned `ERR_NAME_NOT_RESOLVED`.

The issue wasn't the proxy—it was DNS.

I was using an incorrect hostname.

After checking the Environment Group configuration and switching to the assigned `nip.io` hostname, the API became reachable immediately.

---

## 🔗 Continue Exploring

- 🏠 Back to the [Repository Home](../README.md)
- 📖 Read Next: [What is an API Proxy?](../concepts/what-is-a-proxy.md)