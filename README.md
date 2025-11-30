REPORT z_mm_long_001.

*Step1:Define types, variables, constants, internal tables, etc.
TABLES: mara. " Declare a work area with the same name as the database table

ALV parameter declarations
DATA: t_fieldcat TYPE lvc_t_fcat, "Field catalog internal table
w_fieldcat TYPE lvc_s_fcat, "Field catalog work area
w_layout TYPE lvc_s_layo. "Used to define ALV grid layout and attributes

Define type structure
TYPES: BEGIN OF ty_mara,
matnr TYPE matnr, "Material number
pstat LIKE mara-pstat, "Maintenance status
mtart TYPE mtart, "Material type
mbrsh TYPE mbrsh, "Industry sector
matkl TYPE matkl, "Material group
bismt TYPE bismt, "Old material number
meins TYPE meins, "Base unit of measure
brgew TYPE brgew, "Gross weight
ntgew TYPE ntgew, "Net weight
maktx TYPE maktx, "Material description
zline TYPE i, "Current row number
check TYPE c, "Current status
END OF ty_mara.

*Step2： Define the selection screen

Declare internal table and work area
DATA: gt_mara TYPE STANDARD TABLE OF ty_mara,
gs_mara TYPE ty_mara.

Selection screen
SELECTION-SCREEN: BEGIN OF BLOCK bl1 WITH FRAME TITLE TEXT-001.
SELECT-OPTIONS: s_matnr FOR mara-matnr,
s_matkl FOR mara-matkl NO-EXTENSION.
SELECTION-SCREEN SKIP 2.
SELECTION-SCREEN: END OF BLOCK bl1.

*Step3.Retrieve the required data and store it in the internal table

Main program
START-OF-SELECTION. "Subroutine to fetch data
PERFORM get_data. "Subroutine to fetch data
PERFORM set_fieldcat . "Subroutine: Set field attributes
PERFORM set_layout. "Subroutine to set ALV layout
PERFORM display_alv. "Display ALV Report
&---------------------------------------------------------------------
*& Form get_data
&---------------------------------------------------------------------
*& text
&---------------------------------------------------------------------
*& --> p1 text
*& <-- p2 text
&---------------------------------------------------------------------
FORM get_data .

Fetch data by SQL language
SELECT
maramatnr "Material number
pstat "Maintenance status
mtart "Material type
mbrsh "Industry sector
matkl "Material group
bismt "Old material number
meins "Base unit of measure
brgew "Gross weight
ntgew "Net weight
maktx "Material description
FROM mara
LEFT JOIN makt ON maramatnr = maktmatnr AND maktspras = '1'
INTO TABLE gt_mara
WHERE mara~matnr IN s_matnr.

Add row number
LOOP AT gt_mara INTO gs_mara.
gs_mara-zline = sy-tabix.
MODIFY gt_mara FROM gs_mara TRANSPORTING zline.
CLEAR gs_mara.
ENDLOOP.
ENDFORM.

*Step4: Define the list of display fields and their properties (Field Catalogs)

PERFORM set_fieldcat
FORM set_fieldcat .
"定义宏
DEFINE edit_fieldcat.
w_fieldcat-fieldname = &1.
w_fieldcat-scrtext_l = &2.
APPEND w_fieldcat TO t_fieldcat.
CLEAR w_fieldcat.
END-OF-DEFINITION.
w_fieldcat-fieldname = 'MATNR'.
w_fieldcat-scrtext_l = 'Material NO'.
w_fieldcat-ref_table = 'MARA'.
w_fieldcat-ref_field = 'MATNR'.
w_fieldcat-emphasize = 'C100'.
APPEND w_fieldcat TO t_fieldcat.
CLEAR w_fieldcat.

edit_fieldcat: 'PSTAT' 'Maintenance status',
'MTART' 'Material type',
'MBRSH' 'Industry sector',
'MATKL' 'Material group',
'BISMT' 'Old material number',
'MEINS' 'Base unit of measure',
'BRGEW' 'Gross weight',
'NTGEW' 'Net weight',
'MAKTX' 'Material description',
'ZLINE' 'Line number'.

ENDFORM.

*Step5: Define the list of display fields and their properties (Field Catalogs)

PERFORM set_fieldcat
FORM set_layout .
CLEAR w_layout.
w_layout-box_fname = 'CHECK'. "Field in the internal table used for selection
w_layout-sel_mode = 'A'. "Set selection mode
w_layout-cwidth_opt = 'X'. "Optimize column width
w_layout-zebra = 'X'. "Set zebra striping
ENDFORM.
*Step6: Call the function to display ALV data

PERFORM Call the function to display ALV data
FORM display_alv.
CALL FUNCTION 'REUSE_ALV_GRID_DISPLAY_LVC'
EXPORTING
is_layout_lvc = w_layout "ALV layout style
it_fieldcat_lvc = t_fieldcat "ALV display fields
TABLES
t_outtab = gt_mara "Internal table data
EXCEPTIONS
PROGRAM_ERROR = 1
OTHERS = 2.
IF sy-subrc <> 0.

Implement suitable error handling here
ENDIF.
ENDFORM.
