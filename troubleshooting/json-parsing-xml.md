# 🐛 Troubleshooting: "Unexpected token <" While Parsing JSON

**Date:** Dec 6, 2025 | **Context:** Project 3 (API Mashup)

## The Problem

My JavaScript policy failed during execution and the proxy returned a **500 Internal Server Error**.

The error message was:

```text
SyntaxError: Unexpected token <
```

The failure occurred on this line:

```javascript
var data = JSON.parse(context.getVariable("response.content"));
```

At first glance, nothing looked wrong with the JavaScript code.

---

## Investigation

I started checking each part of the request flow.

### ✅ Step 1 — Verify the JavaScript

The syntax was correct.

`JSON.parse()` is the proper way to convert a JSON string into a JavaScript object.

So the problem probably wasn't the code itself.

---

### ✅ Step 2 — Inspect the Backend Response

I checked the backend endpoint.

It was configured as:

```text
mocktarget.apigee.net/xml
```

That immediately raised a question:

> "Why am I parsing JSON if the backend returns XML?"

---

### ✅ Step 3 — Compare the Response Format

JSON typically begins with:

```json
{
```

or

```json
[
```

XML begins with:

```xml
<root>
```

The very first character returned by the backend was:

```text
<
```

That's exactly the character reported in the error message.

---

## Root Cause 🔍

`JSON.parse()` only understands JSON.

It cannot parse XML.

When the JavaScript engine encountered:

```text
<
```

it immediately threw:

```text
SyntaxError: Unexpected token <
```

The parser wasn't broken.

I had supplied the wrong input format.

---

## The Fix

I had two possible solutions.

### Option 1 — Convert XML to JSON

Use an `XMLToJSON` policy before executing the JavaScript policy.

This allows the script to work with JSON objects.

---

### Option 2 — Use a JSON Endpoint

Point the backend to an endpoint that already returns JSON.

For example:

```text
mocktarget.apigee.net/json
```

For this lab, I chose **Option 2** because it kept the proxy simpler.

---

## Why Content-Type Matters

One lesson I learned is that APIs should clearly communicate their response format using the HTTP `Content-Type` header.

Typical examples include:

```text
application/json
```

or

```text
application/xml
```

Checking the response headers before parsing the payload can prevent many integration issues.

---

## Troubleshooting Checklist

Whenever parsing fails, I now check:

- ✅ Is the response actually JSON?
- ✅ What does the `Content-Type` header say?
- ✅ Does the payload begin with `{`, `[`, or `<`?
- ✅ Am I using the correct parser?
- ✅ Should I transform the payload before processing it?

---

## Lesson Learned

One important lesson from this bug was:

> **Never assume the format of a response. Verify it first.**

Many parsing errors are caused by unexpected data formats rather than faulty code.

Always inspect the response before deciding how to process it.

---

## TL;DR

My JavaScript policy failed because I attempted to parse an XML response using `JSON.parse()`.

After identifying that the backend returned XML instead of JSON, I switched to a JSON endpoint for the lab. Alternatively, I could have used an `XMLToJSON` policy to transform the payload before parsing it.

---

## 🔗 Continue Exploring

- 🏠 Back to the [Repository Home](../README.md)
- 📖 Read Next: [What is an API Proxy?](../concepts/what-is-a-proxy.md)