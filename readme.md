# show message by js

`success`

```{.javascript .numberLines}
apex.message.showPageSuccess('sucessMessage')
```

`error`

```js
apex.message.showPageSuccess("sucessMessage");
```

### example:

```js
apex.message.showErrors([
  {
    type: "error",
    location: ["page", "inline"],
    pageItem: "P1_ENAME",
    message: "Name is required!",
    unsafe: false,
  },
  {
    type: "error",
    location: "page",
    message: "Page error has occurred!",
    unsafe: false,
  },
]);
```

---

# APEX ERROR PROCESS

```SQL
apex_error.add_error (
    p_message          => 'This custom account is not active!',
    p_display_location => apex_error.c_inline_in_notification );
```

---

## interactive grid change selected records
```javascipt
let model, record, records, totalRecords, i, view = apex.region("itemTest").call("getCurrentView")
if (view.supports.edit) {

    model = view.model;
    records = view.getSelectedRecords();
    for (i = 0; i < records.length; i++) {
        record = records[i];
        model.setValue(record, "FLG_UPDATE", 'UPD');
    }
}
apex.submit('SEND_TO_FINANCE');
```

## interactive grid active and change all records
```javascipt
let model, record, records, totalRecords, i, view = apex.region("lineT").call("getCurrentView")
if (view.supports.edit) {
    ;
    model = view.model;
    totalRecords = model.getTotalRecords(true)
    for (i = 0; i < totalRecords; i++) {
        record = model.recordAt(i);
        if (record) {
            try { model.setValue(record, 'FLG_ACTION', 'UPD'); } catch (error) {
null;
            }
        }
    }
}
apex.submit('SAVE')
```

---

# HTML EXPRESSION

### IF Direvtive

```HTML
{if STATUS/}
    Active
{elseif !STATUS/}
    Inactive
{else/}
    Unknown
{endif/}
```

---

### case

```html
{case STATUS/} {when ACTIVE/}
<span class="badge badge-success">Active</span>
{when INACTIVE/}
<span class="badge badge-secondary">Inactive</span>
{otherwise/}
<span class="badge badge-light">Unknown</span>
{endcase/}
```

---

# ESCO UPDTE BYPASS

```PLSQL
DECLARE
L_Q VARCHAR2(4000);
L_RES VARCHAR2(4000);
BEGIN
L_Q:= q'~UPDATE LAC_REQ_DOCS S$^T LAC_REQ_EQUIP_ID = :P13_ID ,COD_APP_SESSION = NULL WHERE COD_APP_SESSION = :APP_SESSION~';
L_Q:=REPLACE(L_Q,'$^','E');
EXECUTE IMMEDIATE L_Q USING :P13_ID,:APP_SESSION;

END;
```

---

# POP LOV COLUMN WIDTH

```JS
function(options) {
    const { LIST_LAB: a } = options.columns;
    a.width = '70%';
    return options
}
```

---

# Delete duplicate record in a table

#### `Method 1:`

```sql
DELETE LAB_PARTS P WHERE EXISTS(

SELECT 1 FROM (
    SELECT ROW_NUMBER() OVER(PARTITION BY COD_PART ORDER BY COD_PART) AS ROW_NUM, P.* FROM LAB_PARTS P
)WHERE ROW_NUM > 1
AND ID = P.ID)
```

#### `Method 2:`

```sql
delete lab_parts p where rownum>(select count(*) from lab_parts where cod_part = p.cod_part)
```

---
# SHOW ALL APEX built-in VIEW 
```SQL
select * from APEX_DICTIONARY 
```
for example for show apex share component `List of Values` you shoue use 
```SQL 
select * from APEX_APPLICATION_LOV_COLS
```
---
