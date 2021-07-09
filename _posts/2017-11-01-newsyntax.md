---
title: NEW Syntax
categories: [ABAP]
comments: true
---

### Filter <br>

```sql
DATA: r_filter TYPE SORTED TABLE OF s_carr_id WITH UNIQUE KEY table_line.

CLEAR: r_filter.
APPEND 'AA' TO r_filter.

SELECT *
  FROM sflight
  INTO TABLE @DATA(lt_sflight).

TYPES: ty_sflight TYPE TABLE OF sflight WITH KEY carrid.
DATA(lt_result) = FILTER ty_sflight( lt_sflight IN r_filter WHERE carrid = table_line ).

DATA: lt_result TYPE TABLE OF sflight.
lt_result = FILTER #( lt_sflight IN r_filter WHERE carrid = table_line ).
```