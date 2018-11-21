# Database Class

/progs/mxp/shap102/objects/DB.cls

## Examples

### Read the export date of a shipment

```
define variable dbc          as class DB.
define variable query-string as character no-undo.
define variable export-date  as date     no-undo.

/* init the DB connection */

dbc = new DB("test").

/* set your query */

assign query-string = "for each shiphead where shiphead.id = *".

/* set the ID you're looking for */

dbc:bind("2039337").

/* run the query */

dbc:find("shiphead", query-string).

/* pull the first record */

dbc:next().

/* display the export date */

display date(dbc:get("shiphead", "export-date")).
```

### Update the export date of a shipment

```
define variable dbc          as class DB.
define variable query-string as character no-undo.
define variable export-date  as date     no-undo.

/* init the DB connection */

dbc = new DB("test").

/* set your query */

assign query-string = "for each shiphead where shiphead.id = *").

/* set the ID you're looking for */

dbc:bind("2039337").

/* run the query */

dbc:find("shiphead", query-string).

/* pull the first record */

dbc:next().

/* change the export date */
/* (don't worry, the real export date is actually 9/13/2017) */

dbc:assign("export-date", 09/13/2017).
```

### For each

```
/* query all 40' high cubes on 2039144 */

dbc:bind("2039144").
dbc:bind("4400").

dbc:find("shiphead,bookeq", "for each shiphead where shiphead.id = *, " +
                            "each bookeq where " +
                            "bookeq.branch     = shiphead.branch and " +
                            "bookeq.dept       = shiphead.dept   and " +
                            "bookeq.id         = shiphead.id     and " +
                            "bookeq.equip-type = *").

/* keep pulling containers until you have them all */

do while(dbc:next()):
  message dbc:get("bookeq", "container-number").
end.
```

### Create record

```
/* now to create a record in the notes table */

dbc:create("notes").
dbc:prepare("branch", "01").
dbc:prepare("dept", "02").
dbc:prepare("id", "2044354").
dbc:prepare("seq", 61).
dbc:prepare("note-date", today).
dbc:prepare("note-time", time).
dbc:prepare("note-type", "P").
dbc:prepare("note-text", ".").
dbc:flush().
```

### Delete record

```
/* let's delete the note before anyone sees it */

assign query-string = "for each notes where " +
                               "notes.branch    = * and " +
                               "notes.dept      = * and " +
                               "notes.id        = * and " +
                               "notes.seq       = * and " +
                               "notes.note-text = *".
dbc:bind("01").
dbc:bind("02").
dbc:bind("2044354").
dbc:bind(61).
dbc:bind(".").
dbc:find("notes", query-string).
dbc:next().
dbc:delete().
```

## Methods

To use the DB class, you will:

1.	Create and initialize the DB class into a variable
*	bind() query variables
*	Run find() to run the query itself
*	Call next() to get the first result of your query
*	get() any variables you need

```
define variable dbc as class DB.
dbc = new DB("").
dbc:bind("2039337").
dbc:find("shiphead", "for each shiphead where shiphead.id = *").
dbc:next().
display date(dbc:get("shiphead", "export-date")).
```

There is also support for creating, updating, and deleting records.

### Bind

*   Input
    *   a variable to insert into your query

If you concatenate variables on your query string, you are vulnerable to code injection. To avoid injection, you must use bind().

To use the bind function, just pass in the variable you want. The datatype does not matter. (The bind() method is overridden so that you can pass in any datatype.)

Bind will fill asterisks (*) sequentially. For example, say this is your query:

```
for each shiphead where
         shiphead.cust-no     = * and
         shiphead.create-date = *
```

And you run two binds like this:

```
dbc:bind(“FIVEBMER”).
dbc:bind(01/01/2018).
```

Then the query will resolve to this:

```
for each shiphead where
         shiphead.cust-no     = “FIVEBMER” and
         shiphead.create-date = 01/01/2018
```

Because you ran bind() on FIVEBMER first and 01/01/2018 second.

### Find

*   Input
    *   A comma-delimited list of table names in your query
    *   The query string
        *   For example, "for each shiphead where shiphead.id = *"
*   Output
    *   Success - yes/no

This opens your query. After calling this, call next() to get the results.

It reads the tt-bind temp-table to insert binds where you had asterisks in your query. Then, once the query is fully formed, it's stored in tt-buffer.

### Next

next()

*   Output
    *   Success - yes/no

This will grab the next record of your query.

If next() returns yes, then you can call a getter method to retrieve data about the record.

If next() returns no, there were no more results from your query.

### Getters

#### get, get-logical, get-date, get-integer, get-decimal, get-rowid, get-recid

Use this to get the contents of a field in your queried record.

If your query has one table, you can pass in a lone input parameter: the field name.

```
dbc:get('export-date').
```

If your query has multiple tables, you will need to specify the table, then the field.

```
dbc:get('shiphead', 'export-date').
```

If the field has an extent, you will need to pass that in along with the table and field name.

```
dbc:get('cust-po-lines', 'custom-attr', 2).
```

The get-* functions cast the return value to a certain datatype. get-date, for instance, will cast the output value to a date.

#### get-curr-rowid

*   Input
    *   Table name
*   Output
    * Row ID

Given a table name, this will output the row ID for the record in your query.

This:

```
dbc:bind("2039337").

dbc:find("shiphead", "for each shiphead where shiphead.id = *").

if dbc:next() then do:

  assign shiphead-rowid = dbc:get-curr-rowid("shiphead").

end.
```

Will return the same row ID as this:

```
find first shiphead where
           shiphead.id = "2039337"
           no-lock no-error.

if available shiphead then do:

  assign shiphead-rowid = rowid(shiphead).

end.
```

### Prepare

prepare()

*   Input
    *   table name
    *   field name
    *   new field value to assign
    *   field extent (optional--if you do not have an extent, only pass in 3 input parameters)
*   Output
    *   Success - yes/no

prepare() will queue an update to a record. You will need to call flush() to actually make the update. In this way, you can prepare() multiple updates, then flush() them all at once.

Be warned: if you prepare() an update, next() will flush() your update before moving onto the next record.

### Flush

flush()

*   Output
    *   Success - yes/no

This assigns every update that you have prepare()d.

There are two flush methods: flush() and wait-and-flush().

wait-and-flush() will attempt to flush multiple times before giving up. If the update cannot be made, instead of hanging the program, wait-and-flush() will return no, and the program execution will continue.

Before calling wait-and-flush(), please call set-wait().

### Set Wait

set-wait()

*   Input
    *   email address - who to email if wait-and-flush() fails
    *   timeout 1 - the length of time that wait-and-flush() should try to make an update before sending an email
    *   timeout 2 - the length of time that wait-and-flush() should try to make an update before giving up

### Create

create()

*   Input
    *   table name
*   Output
    *   Success - yes/no

Instead of querying to find a record, this function puts a brand new record in scope.

You will need to call flush() to commit the record to the database. Before then, please prepare() any relevant values to prevent a unique key conflict.

### Delete

delete()

*   Input
    *   table name (optional; if you only have one table in your query, you do not need to pass in an input parameter)
*   Output
    *   Success - yes/no

This deletes the record that is currently in scope.

### Assign

assign()

*   Input
    *   table name
    *   field name
    *   new field value to assign
    *   field extent (optional--if you do not have an extent, only pass in 3 input parameters)
*   Output
    *   Success - yes/no

This will prepare() an update and flush() it immediately.

### Helpers

*	cleanup()
    *	No parameters
    *	If you run a large query, use this after you have pulled all the results you want. This will free up memory associated with the query.
*	clean-str()
    *	Input: a string to clean
    *	Output: the string, cleaned
    *	This makes strings database-safe. It will prevent code injections.
*	get-error()
    *	Input: N/A
    *	Output: an error string
    *	If a method returns false, you can run this to get a message describing what went wrong.
*	get-curr-rowid()
    *	Input: the table name
    *	Output: the row ID
    *	This returns the row ID of the record you currently hold.

