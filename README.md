# Human Language Code (HLC) Explained: A JSON-Based Software Specification Intermediate Language

**1. Purpose**:  
HLC bridges human-readable specifications and structured programming. It defines software requirements using JSON objects, organizing logic into a hierarchical tree. Think of it as a middle layer between natural language (e.g., "Build a calculator app") and technical code.

---

## **Core Structure**

HLC always starts with **two mandatory top-level objects** in an array:

- **Entry-Level Object**: Defines the project’s _structure_ (e.g., framework, rules).
- **Top-Level Object**: Specifies the _type of software_ (e.g., mobile app, game, calculator).  
  These appear first and cannot link to other objects (they are root nodes).

```json
[
  {
    /* Entry-Level Object */
  },
  {
    /* Top-Level Object */
  }
]
```

---

## **Object Types**

Each HLC object has **3 required fields**:

- `name`: A unique identifier (e.g., "DJANGO-STYLE").
- `type`: The role of the object (see below).
- `prompt`: A natural-language explanation of the object’s purpose.

Optional field:

- `linkers`: An array of child objects that refine the parent’s behavior.

---

## **Types of Objects**

1. **Entry-Level** (`type: "entry_level"`):  
   Sets up project-wide structure/rules (e.g., Django project settings).  
   Example: `{ "name": "DJANGO-STYLE", "type": "entry_level", ... }`

2. **Top-Level** (`type: "top_level"`):  
   Defines the software type (e.g., calculator app).  
   Example: `{ "name": "CALCULATOR-APP", "type": "top_level", ... }`

3. **Feature** (`type: "feature"`):  
   Describes functionality (e.g., "file upload"). Linked to parent objects.  
   Example: `{ "name": "FILE_UPLOAD", "type": "feature", ... }`

4. **Control** (`type: "control"`):  
   Dictates behavior (e.g., "auto-save every 5 seconds").

5. **Directive** (`type: "directive"`):  
   Enforces rules (e.g., "use HTTPS"). Often nested under `linkers`.

6. **Literal** (`type: "literal"`):  
   Provides a fixed value (e.g., "version 4.2"). Cannot have `linkers`.

---

## **Linking Objects**

- **Linkers** connect child objects to parents.  
  Each linker has:
  - `name`: The child’s role (e.g., "framework").
  - `description`: How the child affects the parent.
  - `value`: The linked object (e.g., `{ "name": "DJANGO", "type": "directive" }`).

---

## **Example Breakdown**

Let’s analyze the provided HLC snippet:

```json
[
  {
    "name": "DJANGO-STYLE", // Entry-Level Object
    "type": "entry_level",
    "prompt": "Create this project using django style project structure.",
    "linkers": [
      // Links to child objects
      {
        "name": "rules",
        "description": "Project rules",
        "value": {
          // Directive: Split settings files
          "name": "DJANGO-STYLE-RULE",
          "type": "directive",
          "prompt": "Separate the settings file into local, production and testing."
        }
      },
      {
        "name": "framework",
        "description": "Project framework",
        "value": {
          // Directive to use Django
          "name": "DJANGO",
          "type": "directive",
          "prompt": "Use python Django as the framework for this project.",
          "linkers": [
            // Link to a literal
            {
              "name": "version",
              "description": "Django version",
              "value": {
                // Literal: Use latest version
                "name": "VERSION",
                "type": "literal",
                "prompt": "Use the latest version of Django."
              }
            }
          ]
        }
      }
    ]
  },
  {
    "name": "CALCULATOR-APP", // Top-Level Object
    "type": "top_level",
    "prompt": "Create a calculator app.",
    "linkers": [
      {
        "name": "pages",
        "description": "App pages",
        "value": {
          // Literal: Single page
          "name": "PAGES",
          "type": "literal",
          "prompt": "Define a single page for the calculator."
        }
      }
    ]
  }
]
```

### **What This Means**

- **Project Structure**: Uses Django with split settings files (local, production, testing).
- **Framework**: Django (latest version).
- **Software Type**: Calculator app with a single page.

---

## **Key Rules**

1. **Entry-Level + Top-Level First**: Always start with these two objects.
2. **No Cross-Links**: Root objects cannot link to each other.
3. **Literals Are Final**: They provide concrete values and cannot link to other objects.
4. **Hierarchical Flow**: Objects nest via `linkers` to add detail (e.g., a directive links to a literal).

---

## **Use Case**

Imagine writing "Build a blog app with user authentication." In HLC:

1. Entry-Level Object: Define project structure (e.g., "REACT-STYLE").
2. Top-Level Object: "BLOG-APP".
3. Features: "USER_AUTH", "POST_EDITOR".
4. Directives: "Use OAuth2", "Deploy on AWS".

---

## **Why HLC?**

- **Clarity**: Translates vague requirements into structured JSON.
- **Scalability**: Breaks down complex projects into nested components.
- **Machine Readable**: Tools can parse HLC to auto-generate code, docs, or workflows.

By following these rules, anyone can translate human ideas into a machine-friendly specification! 🛠️**Human Language Code (HLC) Explained: A JSON-Based Software Specification Language**
