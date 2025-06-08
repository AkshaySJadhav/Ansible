# 🧩 Ansible Templating – A Quick Guide

Templating in Ansible allows you to dynamically generate files and configurations using Jinja2, a powerful Python-based templating engine.

Referance: https://pypi.org/project/Jinja2/


### 🧠 Why Use Jinja2 in Playbooks?

Jinja2 lets you dynamically generate values using:

1. Variables
2. Conditionals (if, else)
3. Loops (for)
4. Filters (upper, default, join, etc.)

**Examples:**

1. Displaying the Higest number in the given Array:

```
{
  "numbers": [
    13,
    32,
    53,
    34,
    25,
    76,
    17
  ]
}
```

Jinja2 command: `{{ numbers | max }}`
