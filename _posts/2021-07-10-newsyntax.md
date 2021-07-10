---
title: NEW Syntax
categories: [ABAP]
comments: true
---

<br>
내가 자주 사용하는 ABAP New Syntax ᕦ( •ᗜ•)ᕤ <br>
많이 쓸려고 노력중인데... <br>
확실히 전 구문들보다 퍼포먼스도 잘 나오는 느낌 (?)
<br>
<br>
<br>
```abap
SELECT *
  FROM sflight
  INTO TABLE @DATA(lt_sflight).
```

## Filter <br>

```abap
DATA: r_filter TYPE SORTED TABLE OF s_carr_id WITH UNIQUE KEY table_line.

CLEAR: r_filter.
APPEND 'AA' TO r_filter.

"방법1
TYPES: ty_sflight TYPE TABLE OF sflight WITH KEY carrid.
DATA(lt_result) = FILTER ty_sflight( lt_sflight IN r_filter WHERE carrid = table_line ).

"방법2
DATA: lt_result TYPE TABLE OF sflight.
lt_result = FILTER #( lt_sflight IN r_filter WHERE carrid = table_line ).
```

<br>

## CORRESPONDING <br>

```abap
DATA: BEGIN OF ls_result,
        carrid TYPE s_carr_id,
      END OF ls_result,
      lt_result LIKE TABLE OF ls_result.

lt_result = CORRESPONDING #( lt_sflight MAPPING carrid = carrid ).
```
