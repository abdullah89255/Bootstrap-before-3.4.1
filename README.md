# Bootstrap-before-3.4.1
✅ **Yes, that’s correct.**

This is a known **Cross-Site Scripting (XSS) vulnerability** in **Bootstrap** affecting versions:

* **Before 3.4.1**
* **4.x before 4.3.1**

---

### 🧠 Vulnerability Summary

**CVE ID:** [CVE-2019-8331](https://nvd.nist.gov/vuln/detail/CVE-2019-8331)

**Affected components:**

* `tooltip`
* `popover`

**Vulnerable attribute:**
`data-template` (also `data-content`, `title` if HTML is enabled)

---

### 💥 What Causes the XSS

Bootstrap’s Tooltip and Popover components allow developers to specify custom HTML templates via the `data-template` attribute.

Before the patched versions, **the template HTML was not properly sanitized**, so an attacker could inject malicious JavaScript inside the template.

For example:

```html
<button type="button"
  data-toggle="tooltip"
  data-template="<div><script>alert('XSS')</script></div>"
  title="Click me">
  Hover me
</button>
```

When the tooltip is rendered, Bootstrap would insert the malicious HTML into the DOM, executing the `<script>` tag.

---

### 🛠 Fixed Versions

* Bootstrap **3.4.1**
* Bootstrap **4.3.1**

These versions introduced **proper sanitization** of tooltip and popover templates using Bootstrap’s built-in sanitizer (or by escaping untrusted input).

---

### 🔒 How to Mitigate

1. **Upgrade Bootstrap:**

   ```bash
   # For v3
   use bootstrap@3.4.1
   # For v4
   use bootstrap@4.3.1 or newer
   ```

2. **Disable HTML content** unless necessary:

   ```js
   $('[data-toggle="tooltip"]').tooltip({ html: false });
   ```

3. **Enable Sanitization** if using Bootstrap 4.3.1+:

   ```js
   $('[data-toggle="tooltip"]').tooltip({
     sanitize: true
   });
   ```

4. **Never inject untrusted data** into attributes like `data-template`, `data-content`, or `title`.

---

### 🧩 Example Safe Implementation

```html
<button type="button"
  data-toggle="tooltip"
  title="Safe tooltip">
  Hover me
</button>

<script>
  $('[data-toggle="tooltip"]').tooltip({
    html: false,
    sanitize: true
  });
</script>
```

---

Would you like me to show how to **test for this XSS vulnerability** (e.g., using payloads or Burp)?

