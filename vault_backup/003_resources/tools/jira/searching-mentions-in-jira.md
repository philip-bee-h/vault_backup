---
title: searching-mentions-in-jira
created: 2025-01-06
type: resource
category: 
tags:
  - tool-name
  - uiowa
  - reference
  - jira
related_docs:
---
# How to Search for Your @Mentions with JQL

**Date:** July 31, 2024

This guide explains how to use Jira Query Language (JQL) to find issues where you're mentioned or to locate mentions of others. These queries can help in searching, creating filters, configuring dashboard gadgets, automations, and organizing tasks efficiently.

---

## What Are @Mentions?

In Jira, you can mention a person in an issue’s description, comment, or any other text-search-supported field using the format `@username`. Mentions notify users, drawing attention to specific issues.

Under the hood, mentions are stored in the format:

```plaintext
[~accountid:abc123]
```

Here, `abc123` is the Atlassian account ID (AAID) of the user being mentioned.

---

## Searching for Mentions of Yourself

To search for Jira issues that mention you, use the following JQL queries:

### Basic Search for Mentions in Comments

```jql
comment ~ currentUser()
```

This query searches for issues where your username appears in the comments.

### Search for Mentions Updated in the Last Week

```jql
comment ~ currentUser() AND updated >= -7d
```

This query filters issues that mention you and were updated within the past seven days.

### Add a Project Filter for Better Performance

```jql
project IN (FOO, BAR) AND comment ~ currentUser() AND updated >= -7d
```

This query narrows the search to specific projects (e.g., FOO and BAR) to improve performance and focus on relevant tasks.

### Search for Mentions in Any Rich-Text Field

You can search for mentions across fields like `description`, `environment`, and `worklogComment`. Use the `text` field to search across all rich-text fields, including custom fields:

```jql
text ~ currentUser()
```

---

## Searching for Mentions of Someone Else

JQL does not provide autosuggestions for searching mentions of other users. To do this:

1. **Find the User’s AAID**:
    - Navigate to the user's profile in Teams.
    - Copy the ID from the URL (e.g., `/jira/people/abc123`).
2. **Use Their AAID in a Query**:
    - Replace `abc123` with the user’s AAID:

```jql
comment ~ "[~accountid:abc123]"
```


This query finds issues where the user is mentioned in the comments.

---

## Best Practices

- Use **`currentUser()`** for dynamic filters that all teammates can reuse without creating individual filters for each user.
- Always include a **project clause** to improve search performance and relevance.
- Build **dashboard gadgets** or **shared JQL filters** for team-wide efficiency.

---

## Summary

This document covers how to:

- Search for mentions of yourself or others using JQL.
- Narrow searches by time, project, or field.
- Use AAIDs for precise searches.



## Links:
[How to search for your @mentions with JQL](https://community.atlassian.com/t5/Jira-articles/How-to-search-for-your-mentions-with-JQL/ba-p/2771763)