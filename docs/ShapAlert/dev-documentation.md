# ShapTable Developer Documentation

[<< back](/ShapTable/about)

---

## Codebase

ShapTable requires only two files, both found in `js/shaptable/src/`;

* `shap_table.css`
* `shap_table.js`

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

Alternatively, the full init function will run all three at once, plus it prints out the total number of records returned.
```js
function shaptable_full_init( table_name, table_columns, row_entries ){
    shaptable_init(table_name); // init shaptable
	shaptable_header(table_name, table_columns); // insert column headers
    shaptable_fill(table_name, table_columns, row_entries); // insert row data
    shaptable_total_entries( table_name, row_entries ); // prints number of records.
}
```


---

### Table
ShapTable expects a div element by ID to initialize in.
```html
<div id="example_table"></div>
<script>
    shaptable_init(example_table);
</script>
```

---

### Columns / Headers
ShapTable uses an object array, expecting 3 specific key:value pairs `index`, `header`, and `func`.

The `index` must represent the returning parameter from the WSDL call, the `header` can be anything we want the column label to be, And `func` should default to null for most instances. More on `func` options below.

**Example of static array:**
```js
var quote_columns = [
    {
        'index':  'chShapiroID',
        'header': 'Shapiro ID',
        'func':   getShipmentDetails,
    },
    {
        'index':  'quote-status',
        'header': 'Quote Status',
        'func':   null,
    },
    {
        'index':  'pre-ship-id',
        'header': 'Quote Request #',
        'func':   null,
    },
    ...
]
```

--

If you're dealing with a table that has user-defined columns, you'll want to use an iterative loop, 

**Example of generated array;**
```js
    let table_columns = []; // prep an empty array
	for( i = 0; i < user_defined_columns.length; i++ ) {
		let single_column = {}; // prep an empty object

		single_column = { // column headers must follow the shaptable structure.
			'index':  user_defined_columns[i]["field-name"],
			'header': user_defined_columns[i]["fields"]["label"],
			'func':   hasFunc(user_defined_columns[i]["field-name"]), // fill these based on a switchcase for the index value.

		};
        table_columns.push(single_column);
    }
    ...
```

--

The Column's func is intended to specialy affect how the cell's in that column will display, from as simple as wrapping the cell in a link, or replacing with dropdowns that have their own various callbacks and further functionality.

In a static array, you should be able to just write in the function name per object. In a generated array, you'll want to use a switchcase or similar to specify when to call what function.

**Common Example - Wrapping ShapiroID in a link**

```js
function getShipmentDetails(cell, col, row) {
    cell.innerHTML = '&nbsp;<br /><span class="fake_link_orange" onclick="view_popup(&quot;/index.php?option=com_content&amp;view=article&amp;id=235&amp;catid=81&amp;appCode=IT&amp;shapiro_id=' + row['chShapiroID'] + '&quot;);">' + row['chShapiroID'] + '</span><br />&nbsp;';
}
```
A function passed through to the column's `func` property will be applied to all row cells. In this example, all cells in the `chShapiro` column will return a popup link.

---

### Rows / Entries

---

## Under the Hood

### Targeting an entire row
All cells in a row share 2 unique identifiers. 

First, they each have the same class, `row_%n`, where %n is the number. Second, they each have a data-attribute, `data-row="%n"`. 

While targeting by data-attributes allow one to ignore dealing with classes, you should avoid bloating an element with too many attributes. 