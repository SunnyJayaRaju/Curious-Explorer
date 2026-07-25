# 🐛 Troubleshooting: GitHub Actions "Node.js 16 Deprecation"

**Date:** Dec 8, 2025 | **Context:** Project 4 (CI Pipeline)

## The Problem

My GitHub Actions workflow had been working correctly.

Then, without changing my own code, the pipeline suddenly failed during the **Upload Artifact** step.

GitHub reported:

```text
This request has been automatically failed because it uses
a deprecated version of actions/upload-artifact: v3.
```

At first, this was confusing because I hadn't modified the workflow.

---

## Investigation

I checked the workflow step where the failure occurred.

```yaml
- uses: actions/upload-artifact@v3
```

The YAML syntax looked correct.

The workflow structure hadn't changed.

The failure wasn't caused by my pipeline logic.

Instead, the error message pointed directly to the version of the GitHub Action being used.

---

## Root Cause 🔍

GitHub Actions are reusable automation components maintained by GitHub and the community.

Like any software dependency, they evolve over time.

The version I was using:

```yaml
actions/upload-artifact@v3
```

had been deprecated because it relied on an older runtime (Node.js 16), which GitHub no longer supports on its hosted runners.

The workflow itself wasn't broken.

One of its dependencies had reached the end of its supported lifecycle.

---

## The Fix

I updated the workflow to use the current supported version.

Before:

```yaml
- uses: actions/upload-artifact@v3
```

After:

```yaml
- uses: actions/upload-artifact@v4
```

After committing the change, the workflow completed successfully.

---

## Why This Matters

CI/CD pipelines are software too.

Even if application code never changes, automation dependencies continue to evolve.

Keeping workflow actions up to date helps:

- Maintain compatibility with GitHub-hosted runners.
- Receive security and bug fixes.
- Take advantage of performance improvements.
- Avoid unexpected pipeline failures caused by deprecated components.

---

## Troubleshooting Checklist

Whenever a GitHub Actions workflow suddenly fails, I now check:

- ✅ Which workflow step failed?
- ✅ Which GitHub Action version is being used?
- ✅ Has the action been deprecated?
- ✅ Are there newer supported releases?
- ✅ Do the release notes mention breaking changes?

Reading the error message carefully often points directly to the solution.

---

## Lesson Learned

One lesson changed how I think about automation:

> **CI/CD pipelines require maintenance just like application code.**

A stable pipeline today may fail tomorrow if one of its dependencies becomes unsupported.

Keeping automation up to date is part of maintaining reliable software delivery.

---

## TL;DR

My workflow failed because it referenced a deprecated version of `actions/upload-artifact`.

Updating the workflow from `v3` to `v4` restored compatibility with GitHub's hosted runners and allowed the pipeline to complete successfully.