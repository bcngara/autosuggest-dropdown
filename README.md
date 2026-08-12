Markdown

# AutoSuggest Component with Hash Caching & Lazy Record Fallbacks

A high-performance Vanilla JavaScript AutoSuggest component designed to replace heavy HTML `<select>` dropdowns and eliminate user workflow context-switching.

## The Problem

Large database datasets (such as enterprise customer lists or product catalogs) cause two major usability issues in web applications:
1. **DOM Bloat & Performance:** Rendering thousands of `<option>` elements in an HTML `<select>` dropdown degrades browser memory and causes layout rendering lag.
2. **Workflow Interruption:** When creating transactional documents (e.g., invoices or quotations), forced selection requires users to abandon their current form to go register a new entity if it doesn't already exist in the database.

## The Solution

This component addresses both issues with two core architectural patterns:

### 1. Hash-Reconciliation Options Caching
Instead of downloading thousands of options on every keystroke, the client posts its local data hash to the server. If the dataset hasn't changed on the backend, the server returns an lightweight `{ hasUpdate: false }` response, allowing the component to instantly perform client-side filtering on cached local arrays.

### 2. Non-Blocking Lazy Entity Creation
The input operates with dual state representation: a visible text input for human-readable labels, and a hidden input for the database primary key (`id`).
```text
+-------------------------------------------------------------+
| Customer Name: [ ACME Corp          ] (text input)          |
|                [ customer_id = ""   ] (hidden input)        |
+-------------------------------------------------------------+
```

* **Selected Entity:** Choosing a record populates both the text input (`ACME Corp`) and the hidden input (`customer_id = 42`).
* **Unlisted / New Entity:** Typing an unlisted name leaves `customer_id` blank while retaining the text value (`ACME Corp`).
* **Fallback Resolution:** The parent record (e.g., Quotation) stores `customer_id = NULL` alongside the raw string `customer_name = "ACME Corp"`. The quotation can be saved, printed, or emailed immediately without breaking flow. The actual customer profile can be formally registered later and retroactively linked.

---

## File Structure

```text
├── AutoSuggest.js      # Core component class
├── styles.css          # Optional default styling
└── README.md
