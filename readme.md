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

# Apex Collection

#### `CREATE_OR_TRUNCATE_COLLECTION `

```sql
Begin
    APEX_COLLECTION.CREATE_OR_TRUNCATE_COLLECTION(
        p_collection_name => 'EMPLOYEES');
End;
```

<br>

#### `CREATE_COLLECTION_FROM_QUERY Procedure`

Use this procedure to create a collection from a supplied query. The query is parsed as the application owner. This method can be used with a query with up to 50 columns in the SELECT clause. These columns in the SELECT clause populates the 50 character attributes of the collection (C001 through C050). If a collection exists with the same name for the current user in the same session for the current Application ID, an application error is raised.

```sql
Begin
    l_query := 'select make, model, year from AUTOS';
    APEX_COLLECTION.CREATE_COLLECTION_FROM_QUERY (
        p_collection_name => 'AUTO',
        p_query => l_query,
        p_generate_md5 => 'YES');
End;
```

<br>

#### `ADD_MEMBER Function`

```sql
Begin
DECLARE
    l_seq number;
BEGIN
    l_seq := APEX_COLLECTION.ADD_MEMBER(
                 p_collection_name => 'GROCERIES'
                 p_c001            => 'Grapes',
                 p_c002            => 'Imported',
                 p_n001            => 125,
                 p_d001            => sysdate );
END;
End;
```

<br>

### `UPDATE_MEMBER Procedure`

```sql
BEGIN;
    APEX_COLLECTION.UPDATE_MEMBER (
        p_collection_name => 'Departments',
        p_seq => '2',
        p_c001 => 'Engineering',
        p_c002 => 'Sales');
```

<br>

#### `DELETE_MEMBER Procedure`

```sql
APEX_COLLECTION.DELETE_MEMBER (
    p_collection_name IN VARCHAR2,
    p_seq IN VARCHAR2);
```

<br>

#### `CREATE_COLLECTION Procedure`

```sql
APEX_COLLECTION.CREATE_COLLECTION(
    p_collection_name IN VARCHAR2);
```

<br>

#### `Deleting a Collection`

```sql
APEX_COLLECTION.DELETE_COLLECTION (
    p_collection_name IN VARCHAR2);
```

<br>

#### `DELETE_MEMBERS Procedure`

The following example deletes all members of the collection named 'GROCERIES' where the 5th character attribute is equal to 'APPLE'.

```sql
Begin
    apex_collection.delete_members(
        p_collection_name => 'GROCERIES'
        p_attr_number     => 5,
        p_attr_value      => 'APPLE' );
    Commit;
End;

```

---

### getItemWhenLoad DOM JS

```js
function getItemWhenLoad(selector, cbFunction, maxAttempts = 100) {
  let attempts = 0;

  const check = () => {
    const elements = document.querySelectorAll(selector);

    if (elements.length) {
      cbFunction(elements);
      return;
    }

    if (++attempts < maxAttempts) {
      setTimeout(check, 100);
    } else {
      console.warn(`Element not found: ${selector}`);
    }
  };

  check();
}
```

---

# `Faceted Search`

### if error occured when multi checkbox by count filters use below hint on classic report query

```sql
/*+ MATERIALIZE */
```

### `Show By Default JS`

```js
getItemWhenLoad(".a-FS-facetsDashboard", () => {
  [
    "P243_DARKHAST_GROUP_TITLE",
    "P243_DARKHAST_TYPE_TITLE",
    "P243_STATUS",
    "P243_STATE_CODE_KHANEVAR_TITLE",
    "P243_LOCATION_ID_KHANEVAR_NAME_FC",
    "P243_CREATED_TYPE_TITLE",
    "P243_STATE_CODE_IJAD_KONANDE_TITLE",
    "P243_LOCATION_ID_IJAD_KONANDE_NAME_FC1",
  ].forEach((fcItem) => {
    apex.region("fc").addChart(fcItem, null, { type: "pie" });
  });
  renderMaximizedBtnOnChart();
});
```

### `renderMaximizedBtnOnChart `

#### JS Functions

```js
function renderMaximizedBtnOnChart() {
  const elems = document.querySelectorAll("#chartContainer .a-FS-facetChart ");
  elems.forEach((chart) => {
    const btnContainer = chart.querySelector(".a-FS-facetChart-controls");
    const btn = document.createElement("button");
    btn.type = "button";
    btn.classList.add(
      "a-Button",
      "a-Button--noLabel",
      "a-Button--withIcon",
      "ml-1",
      "btn-maximized",
    );
    const btnIcon = document.createElement("span");
    btnIcon.classList.add("t-Icon", "a-Icon", "icon-maximize");
    btn.append(btnIcon);
    btn.addEventListener("click", () => {
      maximizedChart(chart, elems);
    });
    if (!btnContainer.querySelector(".btn-maximized")) {
      btnContainer.prepend(btn);
    }
  });
}
function maximizedChart(chartElem, chartElemList) {
  if (chartElem.classList.contains("is-maximized")) {
    chartElem.classList.remove("is-maximized");
    document.body.classList.remove("all-overflow-hidden");
  } else {
    [...chartElemList].forEach((chart) => {
      chart.classList.remove("is-maximized");
    });
    chartElem.classList.add("is-maximized");
    document.body.classList.add("all-overflow-hidden");
  }
}
```

#### Maximized Styles

```css
.ml-2 {
  margin-left: 0.5rem;
}
.ml-1 {
  margin-left: 0.25rem;
}
#chartContainer .a-FS-facetChart.is-maximized {
  position: fixed;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  z-index: 9999;
  transition: all 0.4s ease-in-out;
}
```

#### Steps to show spinner on fc

1.  define on page variables
1.  add `Faceted Change` on your fc and execute bellow js code and turn on fire on initialization

    ```js
    if (!fcSpinner) {
      fcSpinner = apex.util.showSpinner();
    }
    ```

1.  add `afterRefreshFc` on your fc and execute bellow js code

    ```js
    if (fcSpinner) {
      fcSpinner.remove();
      fcSpinner = null;
    }
    ```

---

### Optimzer Hints

```sql
/*+ MATERIALIZE LEADING(d) USE_NL(k r wf)*/
```

- LEADING: which table should be drive table
- USE_NL: nested loop

---

### onTabChange

```js
function onTabChange(
  tabContainerId,
  cbOnTabChange,
  targetTabId = null,
  cbOnTargetChanged = null,
) {
  getItemWhenLoad(`#${tabContainerId} a.t-Tabs-link`, (tEA) => {
    const getTabModel = (tabId) => {
      return [...document.querySelectorAll(`#${tabId} a.t-Tabs-link`)].map(
        (item, i) => {
          return {
            seq: i,
            id: item.getAttribute("href").substring(4),
            elem: item,
            isSelected:
              item.getAttribute("aria-selected") === "true" ? true : false,
          };
        },
      );
    };
    let tabModel = getTabModel(tabContainerId);

    tabModel.forEach((tab) => {
      tab.elem.addEventListener("click", () => {
        const newTabModel = getTabModel(tabContainerId);
        const tabActive = tabModel.find((item) => item.isSelected);
        const newTabActive = newTabModel.find((item) => item.isSelected);
        const tabIndexActive = tabActive?.seq;
        const newTabIndexActive = newTabActive?.seq;
        tabModel = newTabModel;
        if (newTabIndexActive != null && tabIndexActive != newTabIndexActive) {
          if (cbOnTabChange) {
            cbOnTabChange(newTabActive);
          }
          if (
            targetTabId != null &&
            cbOnTargetChanged != null &&
            newTabActive.id &&
            newTabActive.id == targetTabId
          ) {
            cbOnTargetChanged(newTabActive);
          }
        }
      });
    });
  });
}
```

---

# toggleCollapsibleRegion

```js
/**
 * Toggles or sets the expand/collapse state of a Static Region.
 *
 * @param {string} regionId - The Static ID of the region.
 * @param {'toggle' | 'collapsed' | 'expand'} [type='toggle'] - Operation type. Defaults to 'toggle'.
 * @returns {void}
 */
function toggleCollapsibleRegion(regionId, type = "toggle") {
  const te = document.getElementById(regionId);

  if (!te) {
    console.warn(`Region with Static ID "${regionId}" was not found.`);
    return;
  }

  const teActionBtn = te.querySelector(".t-Button--hideShow");

  if (!teActionBtn) {
    console.warn(`Hide/Show button not found in region "${regionId}".`);
    return;
  }

  const isTeCollapsed = teActionBtn.classList.contains("is-collapsed");

  if (type === "toggle") {
    teActionBtn.click();
  } else if (type === "collapsed" && !isTeCollapsed) {
    teActionBtn.click();
  } else if (type === "expand" && isTeCollapsed) {
    teActionBtn.click();
  }
}
```

# Classic Report Contextual style

```css
.t-ContextualInfo {
  row-gap: 0.5rem;
}
.t-ContextualInfo .t-ContextualInfo-item {
  width: 25%;
  margin: 0;
  display: flex;
  gap: 0.5rem;
}
.t-ContextualInfo .t-ContextualInfo-label::after {
  content: ":";
}
```
