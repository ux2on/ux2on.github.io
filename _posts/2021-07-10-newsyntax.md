---
title: NEW Syntax
categories: [ABAP]
comments: true
---

내가 자주 사용하는 ABAP New Syntax 모음 ᕦ( •ᗜ•)ᕤ

---

```abap
SELECT *
  FROM sflight
  INTO TABLE @DATA(lt_sflight).
```

---

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

<br>

## COND <br>

```abap
IF lt_sflight IS NOT INITIAL.
  DATA(ls_sflight) = lt_sflight[ carrid = 'JL'  connid = 0407	fldate = 20200721 ].
ENDIF.

IF ls_sflight IS NOT INITIAL.
  DATA(lv_result) = COND string( WHEN ls_sflight-seatsmax_b < ls_sflight-seatsocc_b THEN 'OVER'
                                 WHEN ls_sflight-seatsmax_b = ls_sflight-seatsocc_b THEN 'FULL'
                                 WHEN ls_sflight-seatsmax_b > ls_sflight-seatsocc_b THEN 'LESS' ).
ENDIF.
```
