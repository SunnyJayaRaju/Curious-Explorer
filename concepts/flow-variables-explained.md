# 🧠 Flow Variables: The Shared Memory of an API Request

**Date:** Nov 27, 2025 | **Mood:** 🕵️‍♂️ Understanding the Flow

## The Concept

Every request that enters Apigee creates its own execution context.

As the request moves through the API proxy, different policies can:

- Read information
- Create new information
- Update existing information
- Make decisions based on that information

This shared information is stored in **Flow Variables**.

Without Flow Variables, policies would work in isolation and couldn't communicate with each other.

---

## The Analogy: The Medical Clipboard 📋

Imagine a patient arriving at a hospital.

The patient moves through several departments.

### 👩‍⚕️ Reception

The receptionist records:

- Name
- Age
- Patient ID

These notes are written onto the patient's clipboard.

---

### 🩺 Doctor

The doctor doesn't ask for the patient's name again.

Instead, they simply read the clipboard.

The doctor then adds:

```
diagnosis = Flu
```

---

### 🏥 Laboratory

The laboratory adds:

```
blood_test = Normal
```

---

### 💊 Pharmacy

The pharmacist reads everything already written and prepares the medicine.

Nobody keeps asking the patient for the same information because everyone shares the same clipboard.

The clipboard represents the **Flow**.

Each note represents a **Flow Variable**.

---

## Built-in Flow Variables

Apigee automatically creates hundreds of Flow Variables.

Some examples include:

| Variable | Description |
|----------|-------------|
| `request.header.Authorization` | Authorization header sent by the client |
| `request.queryparam.city` | Query parameter value |
| `request.verb` | HTTP method (GET, POST, PUT...) |
| `request.path` | Requested API path |
| `response.status.code` | Backend response status |
| `client.ip` | Client IP address |

These variables are available without writing any code.

---

## Custom Flow Variables

Policies can also create their own variables.

For example:

```xml
<AssignMessage name="Set-Customer-Type">
    <AssignVariable>
        <Name>customer.type</Name>
        <Value>Premium</Value>
    </AssignVariable>
</AssignMessage>
```

Once created, other policies can immediately use:

```
customer.type
```

This is how policies communicate with each other.

---

## Using Flow Variables in Conditions

One of the most common uses is conditional execution.

Example:

```xml
<Step>
    <Name>Spike-Arrest-Policy</Name>
    <Condition>(client.ip = "192.168.1.5")</Condition>
</Step>
```

Apigee evaluates the condition using Flow Variables.

If the condition is true, the policy executes.

Otherwise, it skips that policy.

---

## My Rule of Thumb

Whenever I think about Flow Variables, I remember:

> **Policies don't talk to each other directly. They communicate through Flow Variables.**

That simple idea explains how almost every Apigee proxy works.

---

## TL;DR

Flow Variables are shared pieces of information created and used while an API request moves through an Apigee proxy.

Some variables are automatically provided by Apigee, while others are created by policies. Together they allow policies to share data, make decisions, and coordinate request processing without modifying the original request or response.