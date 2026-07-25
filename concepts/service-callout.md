# 📡 Service Callouts: Making a Side Trip During API Processing

**Date:** Dec 6, 2025 | **Mood:** 🤹 Orchestrating

## The Problem

Most API proxies forward a request to a single backend service.

```
Client
   ↓
Apigee
   ↓
Backend Service
```

But real-world applications are rarely that simple.

Imagine a product page.

To build the complete response, I may need:

- Product details
- Inventory status
- Customer reviews
- Shipping estimate

These pieces of information often come from different backend services.

A single Target Endpoint isn't enough.

---

## The Solution: Service Callout

A **Service Callout** allows an Apigee proxy to make an additional HTTP request while processing the main request.

The proxy:

1. Pauses the current flow.
2. Calls another service.
3. Stores the response in a variable.
4. Continues processing.
5. Returns a single response to the client.

The client never knows multiple backend calls occurred.

---

## The Analogy: Ordering a Pizza 🍕

Imagine I order a Pepperoni Pizza.

The chef discovers there's no pepperoni left.

Instead of cancelling my order, the chef quickly phones a nearby supplier.

```
Customer
    │
    ▼
Pizza Shop
      │
      ├────────► Supplier
      │          │
      ◄──────────┘
      │
      ▼
Customer receives pizza
```

From my perspective as the customer, I placed one order.

Behind the scenes, the pizza shop orchestrated another request to complete it.

A Service Callout works exactly the same way.

---

## Common Use Cases

Service Callouts are useful when an API needs information from another service during request processing.

Examples include:

- Customer profile lookup
- Inventory validation
- Fraud detection
- Credit score verification
- Weather information
- Currency exchange rates
- External authentication services

---

## Understanding `continueOnError`

One setting I found particularly important is:

```xml
continueOnError
```

This determines what happens if the Service Callout fails.

### `continueOnError="false"`

The proxy stops processing immediately.

Use this when the additional service is **required**.

Examples:

- Payment validation
- Identity verification
- Fraud detection

If the service fails, the request should also fail.

---

### `continueOnError="true"`

The proxy continues processing even if the callout fails.

Use this only when the additional service is **optional**.

Examples:

- Product recommendations
- Analytics collection
- Personalisation
- Optional weather information

The user still receives a response, although some information may be missing.

---

## My Rule of Thumb

I ask one simple question:

> **Can my API still produce a valid response without this service?**

- **Yes →** Consider `continueOnError="true"`
- **No →** Use `continueOnError="false"`

The decision depends on business requirements, not the policy itself.

---

## TL;DR

A Service Callout allows an Apigee proxy to temporarily call another HTTP service during request processing.

It's commonly used for orchestration, enrichment, validation, or integration with external systems.

Whether the proxy continues after a failure depends on whether the secondary service is optional or required.