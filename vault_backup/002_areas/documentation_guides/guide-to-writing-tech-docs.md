---
title: Untitled
created: 2025-01-02
type: process
system: 
tags:
  - process
  - procedure
  - documentation
related_docs:
---
# Guide to Writing Effective Tech Documentation

Follow these guidelines to create tech documentation that is clear, actionable, and useful for your audience.

---

## 1. **Write for Beginners**

- Avoid jargon and technical terms that might confuse newcomers.
- Explain concepts in simple, accessible language.

**Example:**  
Instead of:  
_"This function modifies the virtual DOM to re-render components."_  
Use:  
_"This function updates the part of the webpage that has changed without reloading the whole page."_

---

## 2. **Promise a Clear Outcome in the Title**

- Ensure your title directly states what the tutorial achieves.  
    **Example:**  
    _Bad:_ "Learn Docker Basics"  
    _Good:_ "How to Run Your First Docker Container"

---

## 3. **Explain the Goal in the Introduction**

- Clarify the purpose of the tutorial and its benefits.  
    **Example:**  
    _"This tutorial will show you how to deploy a simple web app using Docker, making deployments consistent and reproducible."_

---

## 4. **Show the End Result**

- Include screenshots, diagrams, or descriptions of the final outcome to set clear expectations.

---

## 5. **Make Code Snippets Copy/Pasteable**

- Avoid adding shell prompts (`$`) or line numbers to code snippets.
- Use flags or options to avoid user interaction (e.g., `--yes` instead of prompting).

---

## 6. **Use Descriptive and Consistent Command Options**

- Always use long-form flags for clarity.  
    **Example:**  
    _Bad:_ `grep -r -o '<tag>'`  
    _Good:_ `grep --recursive --only-matching '<tag>'`

---

## 7. **Separate User-Defined Values from Logic**

- Use placeholders or variables to distinguish configurable parts.  
    **Example:**

```bash
API_KEY='your-api-key'
curl --header "Authorization: $API_KEY" https://api.example.com/data
```

---

## 8. **Minimize Dependencies**

- Use the fewest necessary tools and libraries to simplify the setup.
- Specify exact versions for required dependencies.  
    **Example:**  
    _"This tutorial uses Node.js 22.x, tested on v22.12.0 (LTS)."_

---

## 9. **Use Clear and Descriptive Headings**

- Structure content logically with consistent headings.  
    **Example:**  
    _Bad:_  
    "Introduction" → "Step 1" → "Part A"  
    _Good:_  
    "Why Use Docker?" → "Install Docker" → "Deploy Your First Container"

---

## 10. **Demonstrate the Solution Works**

- Show a verification step.  
    **Example:**  
    _"Visit [http://localhost](http://localhost/) to see the default Nginx success page."_

---

## 11. **Keep Code in a Working State**

- Provide working examples at each step and build incrementally.  
    **Example:**  
    Start with a "Hello, World!" example, then add complexity gradually.

---

## 12. **Teach One Thing**

- Focus on a single topic per tutorial to avoid overwhelming readers.

---

## 13. **Avoid Unnecessary Styling**

- Keep examples functional and style-agnostic.  
    **Example:**  
    Use basic HTML/CSS unless styling is the focus.

---

## 14. **Link to Complete Examples**

- Include a link to a repository with all the code. Use CI tools to ensure examples are always up-to-date and functional.

---

By following these principles, your tech documentation will be clear, beginner-friendly, and practical, making it easy for readers to learn and apply the concepts.


From this [LINK](https://refactoringenglish.com/chapters/rules-for-software-tutorials/)
