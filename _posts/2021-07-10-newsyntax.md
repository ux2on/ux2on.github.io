---
title: ABAP New Syntax
categories: [ABAP]
comments: true
---

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

---

## CORRESPONDING <br>

```abap
DATA: BEGIN OF ls_result,
        carrid TYPE s_carr_id,
      END OF ls_result,
      lt_result LIKE TABLE OF ls_result.

lt_result = CORRESPONDING #( lt_sflight MAPPING carrid = carrid ).
```

---

## READ TABLE <br>

### (1) INDEX <br>

```abap
DATA(ls_sflight) = lt_sflight[ 1 ].
```

### (2) LINE INDEX <br>

```abap
DATA(lv_index) = line_index( lt_sflight[ carrid = 'AA' ] ).
```

### (3) WITH KEY <br>

```abap
ls_sflight = lt_sflight[ carrid = 'AA' connid = 0017 ].
```

### (4) LINE EXISTS <br>

```abap
IF line_exists( lt_sflight[ carrid = 'ZZ' ] ).

ENDIF.
```

---

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

---

## SWITCH <br>

```abap
  DATA(ls_sflight) = lt_sflight[ carrid = 'JL'  connid = 0407	fldate = 20200721 ].
ENDIF.

IF ls_sflight IS NOT INITIAL.
  DATA(lv_result) = SWITCH #( ls_sflight-currency
                              WHEN 'KRW' THEN 'KOREA'
                              WHEN 'USD' THEN 'US'
                              WHEN 'JPY' THEN 'JAPAN' ).
ENDIF.
```

---

## VALUE <br>

```abap
TYPES: BEGIN OF ty_new,
         field1 TYPE char10,
         field2 TYPE char10,
         field3 TYPE char10,
         field4 TYPE char10,
         field5 TYPE char10,
       END OF ty_new.
DATA: lt_data TYPE TABLE OF ty_new.

lt_data = VALUE #(

"첫번째 줄
( field1 = '1A' field2 = '1B' field3 = '1C' field4 = '1D' field5 = '1E' )

"두번째 줄
( field1 = '2A' field2 = '2B' field3 = '2C' field4 = '2D' field5 = '2E' )

  field1 = 'Fixed'
( field2 = '3B' field3 = '3C' field4 = '3D'  field5 = '3E' )
( field2 = '4B' field3 = '4C' field4 = '4D'  field5 = '4E' )            ).
```

<br>

### (1) INSERT VALUE <br>

```abap
INSERT VALUE #( field1 = '5A' field2 = '5B' field3 = '5C' field4 = '5D' field5 = '5E' )
  INTO TABLE lt_data.
```

<br>
