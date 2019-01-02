# Automated Document Packages

// add more info later.

### New Database Tables and Fields
```abl
customer-addl.create-doc-package //

billing.produce-doc-package //

invoice.produce-doc-package //

cust-doc-order.
  cust-no
  seq 
  order // the position in the order
  shapiro-doc-code // should only match to shapiro-docs-codes.docs-code
  received-doc-code // user-written example of how clients/vendors name this doc type when sent to us.

shapiro-docs-codes.
  cust-no
  docs-code // the doc code, reference for cust-doc-order.shapiro-doc-code
  docs-code-description // a description of the type of document

```

### Valid Document Naming Standards

In order for the Automated Document Packages suite of programs to work correctly, we need a new naming structure:

General Examples:
```
<Shapiro File ID>-<Doc Code>-<HBL# or Supplier Name>-<remainder of doc name>.<file extension>
basic example.	1234567-CINV.pdf
hbl example.	1234567-CINV-123456.pdf
supplier ex.	1234567-CINV-9876.pdf
```

`imp/isADPValid.p` basic program that takes raw document filename as input and returns a boolean for whether its valid or not. Should also return a more verbose status message.

### Renaming Logic

The parser that renames MRDOCS and DAMCODOCS files, and the Hub360 renamer should both follow the above naming standard moving forward.

