# ShapTable Developer Documentation

[<< back](/ShapAlert/about)

---

## Codebase

ShapTable requires only two files, both found in `js/shapalert/src/`;

* `shapalert.css`
* `shapalert.js`

A third and optional file would be a table-specific init file, to be stored in `js/shaptable/inits`. Example: `js/shaptable/inits/init_po_inquiry.js`.

---

## Basic Usage

The three main, required, components to any Shap Table;
```js
    shaptable_init(table_name); // initialize the table
    shaptable_header(table_name, table_columns); // create the column headers
    shaptable_fill(table_name, table_columns, row_entries); // fill the table with entries
```
This method offers opportunities to control the data going in at each step.

---
