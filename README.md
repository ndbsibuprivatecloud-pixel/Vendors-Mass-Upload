# Vendors-Mass-Upload
Vendors Mass Upload Program

*&---------------------------------------------------------------------*
*& Report zmm_bp
*&---------------------------------------------------------------------*
*&
*&---------------------------------------------------------------------*
REPORT zmm_bp.

TYPES:BEGIN OF ty_vend,
        partn_cat       TYPE  bu_type,
        bpartner        TYPE  bu_partner,
        partn_grp       TYPE  bu_group,
        title_key       TYPE  ad_title,
        name1           TYPE  bu_nameor1,
        name2           TYPE  bu_nameor2,
        name3           TYPE  bu_nameor3,
        name4           TYPE  bu_nameor4,
        searchterm1     TYPE  bu_sort1,
        street          TYPE  ad_street,

        house_no        TYPE  ad_hsnm1,

        street4         TYPE ad_strspp3,
        street5         TYPE ad_lctn,
        district        TYPE ad_city2,

        postl_cod1      TYPE  ad_pstcd1,
        city            TYPE  ad_city1,
        country         TYPE  land1,
        region          TYPE  regio,
        telephone       TYPE  ad_tlnmbr,
        fax             TYPE  ad_fxnmbr,
        e_mail          TYPE  ad_smtpadr,
        j_1ipanno       TYPE  j_1ipanno,
        bkvid           TYPE  bu_bkvid,
        bank_ctry       TYPE  banks,
        bank_key        TYPE  bankk,
        banka           TYPE  banka,
        bank_acct       TYPE  bankn,
        ctrl_key        TYPE  bkont,
        accountholder   TYPE  bu_koinh,
        bukrs           TYPE  bukrs,
        akont           TYPE  akont,            "Reconcilation Account
        zuawa           TYPE  dzuawa,
        altkn           TYPE  altkn,
        zterm           TYPE  dzterm,
        reprf           TYPE  reprf,
        zwels           TYPE  dzwels,
        zahls           TYPE  dzahls,
        witht           TYPE  witht,
        wt_withcd       TYPE  wt_withcd,
        wt_subjct       TYPE  wt_subjct,
        qsrec           TYPE  wt_qsrec,
        wt_wtstcd       TYPE  wt_wtstcd,
        wt_exnr         TYPE  wt_exnr,
        wt_exrt         TYPE  wt_exrt,
        wt_wtexrs       TYPE  wt_wtexrs,
        wt_exdf         TYPE  wt_exdf,
        wt_exdt         TYPE  wt_exdt,
        fiwtin_exem_thr TYPE  fiwtin_exem_thr,

*                                                     start of added by shivam
        cert_id     type IDSAU_CR_ID,
        cert_no     type IDSAU_CR_NO,
        valid_from  type IDSAU_CR_VALID_FR,
        valid_to    type IDSAU_CR_VALID_TO,
        reg_city    type IDSAU_CR_REG_CITY,
*                                                     end of added by shivam

        TYPE            type bu_id_type,
        idnumber        TYPE bu_id_number,
        taxtype         TYPE  bptaxtype,
        gstn            TYPE  pca_cgstn,
        ven_class       TYPE  j_1igtakld,
        ekorg           TYPE  ekorg,
        waers           TYPE  bstwa,
        zterm_p         TYPE  dzterm,
        inco1           TYPE  inco1,
*        inco2           TYPE  inco2,
        inco2           TYPE  inco2_l,
        kalsk           TYPE  kalsk,
        verkf           TYPE  everk,
        telf1           TYPE  telfe,
        webre           TYPE  webre,
        lebre           TYPE  lebre,
        index           TYPE  i,
        waers1          TYPE  waers,
        wt_exnr1        TYPE  wt_exnr,
        wt_exrt1        TYPE  wt_exrt,
        wt_exdf1        TYPE  wt_exdf,
        wt_exdt1        TYPE  wt_exdt,
        wt_wtexrs1      TYPE  wt_wtexrs,
        witht1          TYPE  witht,
        wt_withcd1      TYPE  wt_withcd,
        partnrole       TYPE  bu_role,
        seccode         TYPE  secco,
      END OF ty_vend,


      BEGIN OF ty_bp_log,
        bp_lifnr TYPE bu_partner,
        bpname   TYPE bu_descrip,
        uzeit    TYPE sy-uzeit,
        erdat    TYPE datum,
        uname    TYPE sy-uname,
        msg      TYPE  bapi_msg,
        msgnr    TYPE symsgno,
        msgtyp   TYPE bapi_mtype,
      END OF ty_bp_log,

      BEGIN OF ty_withtax,
        lifnr      TYPE lifnr,
        bukrs      TYPE bukrs,
        witht      TYPE lfbw-witht,
        wt_withcd  TYPE lfbw-wt_withcd,
        wt_subjct  TYPE lfbw-wt_subjct,
        qsrec      TYPE lfbw-qsrec,
        witht2     TYPE lfbw-witht,
        wt_withcd2 TYPE lfbw-wt_withcd,
        wt_subjct2 TYPE lfbw-wt_subjct,
        qsrec2     TYPE lfbw-qsrec,
      END OF ty_withtax.



TABLES:lfa1,but000,tb001.

DATA:gt_vendor                TYPE STANDARD TABLE OF ty_vend,
     lt_vendor                TYPE STANDARD TABLE OF ty_vend,
     wa_vendor                TYPE ty_vend,
     lw_vendor                TYPE ty_vend,

     it_lfa1                  TYPE STANDARD TABLE OF lfa1,

     "BAPI Realted Declarations
     businesspartner          TYPE bapibus1006_head-bpartner,
     businesspartnerextern    TYPE bapibus1006_head-bpartner,
     partnercategory          TYPE bapibus1006_head-partn_cat,
     partnergroup             TYPE bapibus1006_head-partn_grp,
     centraldata              TYPE bapibus1006_central,
     centraldatax             TYPE bapibus1006_central_x,
     centraldataperson        TYPE bapibus1006_central_person,
     centraldatapersonx       TYPE bapibus1006_central_person_x,
     centraldataorganization  TYPE bapibus1006_central_organ,
     centraldataorganizationx TYPE bapibus1006_central_organ_x,
     addressdata              TYPE bapibus1006_address,
     addressdatax             TYPE bapibus1006_address_x,



     it_telephondata          TYPE STANDARD TABLE OF bapiadtel,
     wa_telephondata          TYPE bapiadtel,
     it_faxdata               TYPE STANDARD TABLE OF bapiadfax,
     wa_faxdata               TYPE bapiadfax,
     it_maildata              TYPE STANDARD TABLE OF bapiadsmtp,
     wa_maildata              TYPE bapiadsmtp,


     it_maildatax             TYPE STANDARD TABLE OF bapiadsmtx,
     wa_maildatax             TYPE bapiadsmtx,

     it_teleph_datax          TYPE STANDARD TABLE OF  bapiadtelx,
     wa_teleph_datax          TYPE bapiadtelx,






     it_return                TYPE STANDARD TABLE OF bapiret2,
     wa_return                TYPE  bapiret2,
     it_log                   TYPE STANDARD TABLE OF ty_bp_log,
     wa_log                   TYPE ty_bp_log,
     wa_layout                TYPE slis_layout_alv,
     it_fct                   TYPE STANDARD TABLE OF slis_fieldcat_alv,
     wa_fct                   TYPE slis_fieldcat_alv,
     vend                     TYPE c,
*     CUST                     TYPE C,
     var1                     TYPE c,
     index                    TYPE n,
     total(2)                 TYPE n,
     bpname                   TYPE bu_partner,
     progres                  TYPE string.



"Classes Creation
CLASS l_data DEFINITION.
  PUBLIC SECTION.
    METHODS:create_vendor_data.

  PRIVATE SECTION.
    ##NEEDED   METHODS: prepare_data_vend       RETURNING VALUE(re_flag) TYPE i.

    "Declarations For Vendor
    DATA: gs_vmds_extern   TYPE vmds_ei_main,
          gs_vmds_succ     TYPE vmds_ei_main,
          gs_succ_messages TYPE cvis_message,
          gs_vmds_error    TYPE vmds_ei_main,
          gs_err_messages  TYPE cvis_message.


ENDCLASS.

DATA: lo_data  TYPE REF TO l_data .

SELECTION-SCREEN BEGIN OF BLOCK b1 WITH FRAME TITLE TEXT-001.
  PARAMETERS:p_file TYPE localfile OBLIGATORY.
SELECTION-SCREEN END OF BLOCK b1.


INITIALIZATION.
  PERFORM sub_init.

AT SELECTION-SCREEN ON VALUE-REQUEST FOR p_file.
  PERFORM sub_f4_help.

AT SELECTION-SCREEN.
  PERFORM sub_validate_file.

START-OF-SELECTION.

  PERFORM sub_data_upload.
  IF vend EQ 'X'.
    PERFORM sub_vendor_create.
  ENDIF.

  PERFORM sub_disp_log.



*&---------------------------------------------------------------------*
*& Form SUB_F4_HELP
*&---------------------------------------------------------------------*
*& F4 Option to Path
*&---------------------------------------------------------------------*

FORM sub_f4_help .
  CALL FUNCTION 'F4_FILENAME'
    EXPORTING
      program_name = sy-repid
    IMPORTING
      file_name    = p_file.
ENDFORM.
*&---------------------------------------------------------------------*
*& Form SUB_DATA_UPLOAD
*&---------------------------------------------------------------------*
*& File Upload
*&---------------------------------------------------------------------*
FORM sub_data_upload .


  DATA: it_raw TYPE truxs_t_text_data.

*  SELECT LIFNR FROM LFA1 INTO TABLE @DATA(VENDOR) UP TO 10 ROWS.
  IF vend EQ 'X'.
    CALL FUNCTION 'TEXT_CONVERT_XLS_TO_SAP'
      EXPORTING
        i_line_header        = 'X'
        i_tab_raw_data       = it_raw
        i_filename           = p_file
      TABLES
        i_tab_converted_data = gt_vendor
      EXCEPTIONS
        conversion_failed    = 1
        OTHERS               = 2.
    IF sy-subrc <> 0.
      MESSAGE 'Please Try Again Error in File Uploading'(015) TYPE 'I'.
      LEAVE LIST-PROCESSING.
    ELSE.
      LOOP AT gt_vendor INTO wa_vendor.
        wa_vendor-seccode = 'MH00'.
        wa_vendor-wt_exnr1 = wa_vendor-wt_exnr.
        wa_vendor-wt_exrt1 = wa_vendor-wt_exrt.
        wa_vendor-wt_exdf1 = wa_vendor-wt_exdf.
        wa_vendor-wt_exdt1 = wa_vendor-wt_exdt.
        wa_vendor-wt_wtexrs1 = wa_vendor-wt_wtexrs.
        wa_vendor-wt_withcd1 = wa_vendor-wt_withcd.
        MODIFY gt_vendor FROM wa_vendor INDEX sy-tabix.
        CLEAR: wa_vendor.
      ENDLOOP.
    ENDIF.

  ENDIF.


ENDFORM.
*&---------------------------------------------------------------------*
*& Form SUB_VENDOR_CREATE
*&---------------------------------------------------------------------*
FORM sub_vendor_create .
  CLEAR:lfa1,var1.

  CREATE OBJECT lo_data.
  DESCRIBE TABLE gt_vendor LINES total.                 "#EC CI_CONV_OK
  LOOP AT gt_vendor INTO wa_vendor.


    CALL FUNCTION 'CONVERSION_EXIT_ALPHA_INPUT'
      EXPORTING
        input  = wa_vendor-bpartner
      IMPORTING
        output = wa_vendor-bpartner.

    bpname = wa_vendor-index.
    index = index + 1.                                  "#EC CI_CONV_OK

    CONCATENATE index '/' total '  Processing'(028) INTO progres.
    CALL FUNCTION 'SAPGUI_PROGRESS_INDICATOR'
      EXPORTING
        percentage = index
        text       = progres.

    PERFORM sub_init_vend.

    IF wa_vendor-searchterm1 IS INITIAL.
      PERFORM sub_log_update USING 'Please Maintain Search Term'(023) '002' 'E'.
      CONTINUE.
    ENDIF.

    IF wa_vendor-name1 IS INITIAL.
      PERFORM sub_log_update USING 'Please Maintain Name'(024) '002' 'E'.
      CONTINUE.
    ENDIF.

    IF wa_vendor-e_mail CA '*&^$%#!~`/\()'.
      PERFORM sub_log_update USING 'Please Maintain Email separator , or ;'(025) '002' 'E'.
      CONTINUE.
    ENDIF.


    "Checking for Accounting Group
    IF wa_vendor-partn_grp IS NOT INITIAL.
      SELECT SINGLE * FROM tb001 INTO @DATA(ls_tb001) WHERE bu_group EQ @wa_vendor-partn_grp.
      IF sy-subrc NE 0.
        PERFORM sub_log_update USING 'Enter a Valid Partner Group'(026) '002' 'E'.
        CONTINUE.
      ENDIF.
    ENDIF.

    PERFORM sub_fill_header.
    PERFORM sub_fill_central.
    PERFORM sub_fill_central_org_person.
    PERFORM sub_fill_address.
    PERFORM sub_fill_tele.
    PERFORM sub_fill_fax.
    PERFORM sub_fill_mail.


    SELECT SINGLE partner "*
      FROM but000
      INTO CORRESPONDING FIELDS OF but000
       WHERE partner =  wa_vendor-bpartner. "#EC CI_SEL_NESTED or "#EC CI_SROFC_NESTED "#EC CI_ALL_FIELDS_NEEDED
    IF sy-subrc NE 0.
      PERFORM sub_create_bp_int.
      PERFORM sub_bp_role.
    ELSE.
      PERFORM sub_change_bp.
    ENDIF.

    "Preparing Vendor Data and Customer Data
    CLEAR lfa1.
    SELECT SINGLE * FROM lfa1 INTO lfa1 WHERE lifnr EQ businesspartner . "#EC CI_SEL_NESTED or "#EC CI_SROFC_NESTED  "#EC CI_SUBRC  "WA_VENDOR-BPARTNER.
    IF businesspartner IS NOT INITIAL.
      CALL METHOD lo_data->create_vendor_data.
      COMMIT WORK..
*      WAIT UP TO 5 SECONDS.
      WAIT UP TO 1 SECONDS.
    ENDIF.

    CLEAR:wa_vendor,progres.
  ENDLOOP.
ENDFORM.
*&---------------------------------------------------------------------*
*& Form SUB_FILL_HEADER
*&---------------------------------------------------------------------*

FORM sub_fill_header .
  CLEAR:businesspartnerextern,partnercategory,partnergroup.

  "Filling Header Data
  IF wa_vendor-partn_grp = 'INTR'.
    businesspartnerextern           =   wa_vendor-bpartner.       "Partner No
  ENDIF.
  IF wa_vendor-partn_cat IS INITIAL.
    partnercategory                 =   '2'.      "Partner Categeory
  ELSE.
    partnercategory                 =   wa_vendor-partn_cat.      "Partner Categeory
  ENDIF.
  partnergroup                    =   wa_vendor-partn_grp.      "Partner Grouop
ENDFORM.
*&---------------------------------------------------------------------*
*& Form SUB_FILL_CENTRAL
*&---------------------------------------------------------------------*

FORM sub_fill_central .
  CLEAR:centraldata,centraldatax.

  "Filling Central Data
  centraldata-searchterm1         =   wa_vendor-searchterm1.    "Search Term1
  centraldata-title_key           =   wa_vendor-title_key.      "Title KEy like Mr or Ms Or Company
  centraldata-partnerlanguage     =   'E'.                      "Language
  centraldata-partnerlanguageiso  =   'EN'.                     "Language

  centraldatax-searchterm1        =   'X'.                      "Search Term1 Update
  centraldatax-title_key          =   'X'.                      "Title KEy Update
  centraldatax-partnerlanguage    =   'X'.                      "Language Update
  centraldatax-partnerlanguageiso =   'X'.                      "Language Update
ENDFORM.
*&---------------------------------------------------------------------*
*& Form SUB_FILL_CENTRAL_ORG_PERSON
*&---------------------------------------------------------------------*

FORM sub_fill_central_org_person .
  CLEAR:centraldataorganization,centraldataorganizationx,centraldataperson,centraldatapersonx.

  "Filling Central data Fields
  IF partnercategory EQ 2.
    centraldataorganization-name1 = wa_vendor-name1.
    centraldataorganization-name2 = wa_vendor-name2.
    centraldataorganization-name3 = wa_vendor-name3.
    centraldataorganization-name4 = wa_vendor-name4.

    centraldataorganizationx-name1 = 'X'.
    centraldataorganizationx-name2 = 'X'.
    centraldataorganizationx-name3 = 'X'.
    centraldataorganizationx-name4 = 'X'.
  ELSEIF partnercategory EQ 1.
    centraldataperson-firstname    = wa_vendor-name1.
    centraldataperson-lastname     = wa_vendor-name2.
    centraldataperson-birthname    = wa_vendor-name3.
    centraldataperson-correspondlanguage    = 'E'.
    centraldataperson-correspondlanguageiso = 'EN'.

    centraldatapersonx-firstname   = 'X'.
    centraldatapersonx-lastname    = 'X'.
    centraldatapersonx-birthname   = 'X'.
    centraldatapersonx-correspondlanguage    = 'X'.
    centraldatapersonx-correspondlanguageiso = 'X'.
  ENDIF.
ENDFORM.
*&---------------------------------------------------------------------*
*& Form SUB_FILL_ADDRESS
*&---------------------------------------------------------------------*

FORM sub_fill_address .
  CLEAR:addressdata,addressdatax.
  "Filling Address Fields
  addressdata-street        = wa_vendor-street   .     "
*  ADDRESSDATA-STR_SUPPL1    = WA_VENDOR-HOUSE_NO  .   "

  addressdata-house_no   = wa_vendor-house_no  .   "
  addressdata-postl_cod1    = wa_vendor-postl_cod1.

  addressdata-str_suppl3  =  wa_vendor-street4.
  addressdata-location    =  wa_vendor-street5.
  addressdata-district    =  wa_vendor-district.



  addressdata-city          = wa_vendor-city      .
  addressdata-country       = wa_vendor-country   .
  addressdata-region        = wa_vendor-region    .
  addressdata-langu         = 'E'.
  addressdata-languiso      = 'EN'.

  addressdatax-street        = 'X'.     "
*  ADDRESSDATAX-STR_SUPPL1    = 'X'.   "
  addressdatax-house_no     = 'X'.   "
  addressdatax-postl_cod1    = 'X'.
  addressdatax-city          = 'X'.
  addressdatax-country       = 'X'.
  addressdatax-region        = 'X'.
  addressdatax-langu         = 'X'.
  addressdatax-langu_iso      = 'X'.
  addressdatax-str_suppl3  =  'X'.
  addressdatax-location    =  'X'.
  addressdatax-district    =  'X'.


ENDFORM.
*&---------------------------------------------------------------------*
*& Form SUB_FILL_TELE
*&---------------------------------------------------------------------*

FORM sub_fill_tele .
*  BREAK-POINT.
  "Filling Telephone Data
  REFRESH it_telephondata.
  CLEAR wa_telephondata.

  REFRESH : it_teleph_datax.
  CLEAR : wa_teleph_datax.

  wa_telephondata-country   = wa_vendor-country.
  wa_telephondata-telephone = wa_vendor-telephone.
  APPEND wa_telephondata TO it_telephondata.


  wa_teleph_datax-telephone = 'X'.
  APPEND wa_teleph_datax TO it_teleph_datax.
  CLEAR wa_teleph_datax.
ENDFORM.
*&---------------------------------------------------------------------*
*& Form SUB_FILL_FAX
*&---------------------------------------------------------------------*

FORM sub_fill_fax .
  "Filling FAX Data
  REFRESH it_faxdata.
  CLEAR wa_faxdata.

  wa_faxdata-fax = wa_vendor-fax.
  APPEND wa_faxdata TO it_faxdata.
  CLEAR wa_faxdata.
ENDFORM.
*&---------------------------------------------------------------------*
*& Form SUB_FILL_MAIL
*&---------------------------------------------------------------------*

FORM sub_fill_mail .
  DATA lv_mail TYPE ad_smtpadr.
  "Filling Mail Data
  REFRESH : it_maildata  .
  CLEAR : wa_maildata.


  REFRESH : it_maildatax.
  CLEAR  : wa_maildatax.

  IF wa_vendor-e_mail CA ',;'.
    IF wa_vendor-e_mail CA ';'.
      SPLIT wa_vendor-e_mail AT ';' INTO wa_maildata-e_mail lv_mail .
    ELSEIF wa_vendor-e_mail CA ','..
      SPLIT wa_vendor-e_mail AT ',' INTO wa_maildata-e_mail lv_mail .
    ENDIF.

    IF wa_maildata-e_mail IS NOT INITIAL.
      APPEND wa_maildata TO it_maildata.
      CLEAR wa_maildata.
    ENDIF.

    IF lv_mail IS NOT INITIAL.
      wa_maildata-e_mail = lv_mail.
      APPEND wa_maildata TO it_maildata.
      CLEAR: wa_maildata,lv_mail.
    ENDIF.
  ELSE.
*    BREAK-POINT.
    wa_maildata-e_mail = wa_vendor-e_mail.
    APPEND wa_maildata TO it_maildata.
    CLEAR wa_maildata.

    wa_maildatax-e_mail = 'X'.
    APPEND wa_maildatax TO it_maildatax.
    CLEAR : wa_maildatax.

  ENDIF.

ENDFORM.

CLASS l_data IMPLEMENTATION.

  "Preparing Data For Vendor Creation
  METHOD prepare_data_vend.

    "Local Declarations
    DATA: lt_vendors   TYPE vmds_ei_extern_t,
          ls_vendors   TYPE vmds_ei_extern,

          ls_address   TYPE cvis_ei_address1,
          lt_bank      TYPE cvis_ei_bankdetail_t,
          ls_bank      TYPE cvis_ei_cvi_bankdetail,
          lss_bank     TYPE cvis_ei_bankdetail,
          lt_company   TYPE vmds_ei_company_t,
          ls_company   TYPE vmds_ei_company,
          lss_company  TYPE vmds_ei_vmd_company,
          lt_tax       TYPE vmds_ei_wtax_type_t,
          ls_tax       TYPE vmds_ei_wtax_type,
          lss_tax      TYPE vmds_ei_wtax_type_s,
          lt_purchase  TYPE vmds_ei_purchasing_t,
          ls_purchase  TYPE vmds_ei_purchasing,
          lss_purchase TYPE vmds_ei_vmd_purchasing.

    "Clearing and Refreshing
    REFRESH:lt_vendors,lt_bank,lt_company,lt_tax,lt_purchase.

    CLEAR:ls_vendors,ls_address,ls_bank,lss_bank,ls_company,lss_company,ls_tax,lss_tax,ls_purchase,lss_purchase.
    """"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
    ##WARN_OK    SELECT  SINGLE vendor INTO ls_vendors-header-object_instance-lifnr "#EC CI_SEL_NESTED or "#EC CI_SROFC_NESTED
            FROM but000 AS a
            INNER JOIN cvi_vend_link AS b
         ON a~partner_guid = b~partner_guid
         WHERE partner = businesspartner.

    IF sy-subrc <> 0.
      CALL FUNCTION 'CONVERSION_EXIT_ALPHA_INPUT'
        EXPORTING
          input  = businesspartner "WA_VENDOR-BPARTNER
        IMPORTING
          output = ls_vendors-header-object_instance-lifnr.
    ELSE.
      SELECT SINGLE * FROM lfa1 INTO lfa1 WHERE lifnr EQ ls_vendors-header-object_instance-lifnr . "#EC CI_SUBRC    "WA_VENDOR-BPARTNER.
    ENDIF.

    ls_vendors-header-object_task = 'M'.
    ls_vendors-central_data-central-data-ktokk = wa_vendor-partn_grp.
    ls_vendors-central_data-central-data-stcd3 = wa_vendor-gstn.
    ls_vendors-central_data-central-data-j_1ipanno = wa_vendor-j_1ipanno.
    ls_vendors-central_data-central-data-ven_class = wa_vendor-ven_class.
    ls_vendors-central_data-central-datax-ktokk = 'X'.
    ls_vendors-central_data-central-datax-stcd3 = 'X'.
    ls_vendors-central_data-central-datax-j_1ipanno = 'X'.
    ls_vendors-central_data-central-datax-ven_class = 'X'.

    "Vendor Central Dtaa Address Data
    ls_address-task                    =  'M'.
    ls_address-postal-data-name       =  wa_vendor-name1.
    ls_address-postal-data-name_2      =  wa_vendor-name2.
    ls_address-postal-data-name_3      =  wa_vendor-name3.
    ls_address-postal-data-name_4      =  wa_vendor-name4.
    ls_address-postal-data-city        =  wa_vendor-city.
    ls_address-postal-data-title       =  wa_vendor-title_key.
    ls_address-postal-data-street      =  wa_vendor-street.
*******JG******************
    ls_address-postal-data-str_suppl3  =  wa_vendor-street4.
    ls_address-postal-data-location    =  wa_vendor-street5.
    ls_address-postal-data-district    =  wa_vendor-district.

    ls_address-postal-data-str_suppl1  =  wa_vendor-house_no.
    ls_address-postal-data-house_no  =  wa_vendor-house_no.
    ls_address-postal-data-postl_cod1  =  wa_vendor-postl_cod1.
    ls_address-postal-data-country     =  wa_vendor-country.
    ls_address-postal-data-langu       =  'E'.
    ls_address-postal-data-langu_iso   =  'EN'.
    ls_address-postal-data-region      =  wa_vendor-region.
    ls_address-postal-data-sort1       =  wa_vendor-searchterm1.

    ls_address-postal-datax-name        =  'X'.
    ls_address-postal-datax-name_2      =  'X'.
    ls_address-postal-datax-name_3      =  'X'.
    ls_address-postal-datax-name_4      =  'X'.
    ls_address-postal-datax-city        =  'X'.
    ls_address-postal-datax-title       =  'X'.
    ls_address-postal-datax-street      =  'X'.
*    LS_ADDRESS-POSTAL-DATAX-STR_SUPPL1  =  'X'.
    ls_address-postal-datax-house_no  =  'X'.
    ls_address-postal-datax-postl_cod1  =  'X'.
    ls_address-postal-datax-country     =  'X'.
    ls_address-postal-datax-langu       =  'X'.
    ls_address-postal-datax-langu_iso   =  'X'.
    ls_address-postal-datax-region      =  'X'.
    ls_address-postal-datax-sort1       =  'X'.

    ls_address-postal-datax-str_suppl3  =  'X'.
    ls_address-postal-datax-location    =  'X'.
    ls_address-postal-datax-district    =  'X'.


    "Company Code Data
    ls_company-task                     = 'M'.
    ls_company-data_key-bukrs           = wa_vendor-bukrs.

    CALL FUNCTION 'CONVERSION_EXIT_ALPHA_INPUT'
      EXPORTING
        input  = wa_vendor-akont
      IMPORTING
        output = ls_company-data-akont.

    ls_company-data-zuawa               = wa_vendor-zuawa.
    ls_company-data-altkn               = wa_vendor-altkn.
    ls_company-data-zterm               = wa_vendor-zterm.
    ls_company-data-reprf               = wa_vendor-reprf.
    ls_company-data-zwels               = wa_vendor-zwels.
    ls_company-data-zahls               = wa_vendor-zahls.

    ls_company-datax-akont               = 'X'.
    ls_company-datax-zuawa               = 'X'.
    ls_company-datax-altkn               = 'X'.
    ls_company-datax-zterm               = 'X'.
    ls_company-datax-reprf               = 'X'.
    ls_company-datax-zwels               = 'X'.
    ls_company-datax-zahls               = 'X'.

**Add WithHolding Tax
*    IF WA_VENDOR-WITHT IS NOT INITIAL.
*      LS_TAX-TASK = 'M'.
*      LS_TAX-DATA_KEY = WA_VENDOR-WITHT.                "#EC CI_CONV_OK
*      LS_TAX-DATA-WT_WITHCD = WA_VENDOR-WT_WITHCD.
*      LS_TAX-DATA-WT_SUBJCT = WA_VENDOR-WT_SUBJCT.
*      LS_TAX-DATA-QSREC     = WA_VENDOR-QSREC.
*      LS_TAX-DATA-WT_WTSTCD = WA_VENDOR-WT_WTSTCD.
*
*      LS_TAX-DATAX-WT_WITHCD = 'X'.
*      LS_TAX-DATAX-QSREC     = 'X'.
*      LS_TAX-DATAX-WT_SUBJCT = 'X'.
*      LS_TAX-DATAX-WT_WTSTCD = 'X'.
*
*      APPEND LS_TAX TO LT_TAX.
*      LSS_TAX-WTAX_TYPE = LT_TAX[].
*      LS_COMPANY-WTAX_TYPE = LSS_TAX.
*    ENDIF.
*
    APPEND ls_company TO lt_company.
    lss_company-company = lt_company[].                 "#EC CI_CONV_OK



    "Purchasing Organization Data
    ls_purchase-task                      = 'M'.
    ls_purchase-data_key-ekorg            = wa_vendor-ekorg.
    ls_purchase-data-waers                = wa_vendor-waers.
*    LS_PURCHASE-DATA-ZTERM                = WA_VENDOR-ZTERM. "Commented by swapna on 15-11-19
    ls_purchase-data-zterm               = wa_vendor-zterm_p." Addded by swapna on 15-11-19
    ls_purchase-data-inco1                = wa_vendor-inco1.
*    ls_purchase-data-inco2                = wa_vendor-inco2.
    ls_purchase-data-inco2_l              = wa_vendor-inco2. " incoterm location1
    ls_purchase-data-kalsk                = wa_vendor-kalsk.
    ls_purchase-data-verkf                = wa_vendor-verkf.
*    BREAK-POINT.
    ls_purchase-data-telf1                = wa_vendor-telf1.
*    LS_PURCHASE-DATA-TELF1                = WA_VENDOR-TELEPHONE.

    ls_purchase-data-webre                = wa_vendor-webre.
    ls_purchase-data-lebre                = wa_vendor-lebre.


*                                                                         start of added by shivam

    DATA lt_partner_role TYPE TABLE OF vmds_ei_functions.
    DATA ls_partner_role TYPE vmds_ei_functions.

    ls_partner_role-task = 'I'.
    ls_partner_role-data-partner = lfa1-lifnr.
    ls_partner_role-datax-partner = 'X'.
    ls_partner_role-data_key-parvw = 'LF'.

    APPEND ls_partner_role TO lt_partner_role.

    ls_purchase-functions-functions = lt_partner_role.

*                                                                           end of added by shivam

    ls_purchase-datax-waers                = 'X'.
    ls_purchase-datax-zterm                = 'X'.
    ls_purchase-datax-inco1                = 'X'.
    ls_purchase-datax-inco2                = 'X'.
    ls_purchase-datax-kalsk                = 'X'.
    ls_purchase-datax-verkf                = 'X'.
    ls_purchase-datax-telf1                = 'X'.
    ls_purchase-datax-webre                = 'X'.
    ls_purchase-datax-lebre                = 'X'.
    APPEND ls_purchase TO lt_purchase.
    lss_purchase-purchasing    = lt_purchase.           "#EC CI_CONV_OK


    ls_vendors-central_data-address    = ls_address.
    ls_vendors-company_data            = lss_company.
    ls_vendors-purchasing_data         = lss_purchase.

    APPEND ls_vendors TO lt_vendors.

    gs_vmds_extern-vendors = lt_vendors[].              "#EC CI_CONV_OK

    """"""""""""""""""""""""""""""""""
    PERFORM sub_bank_details.
    """"""""""""""""""""""""""""""""""
    WAIT UP TO 1 SECONDS   .
  ENDMETHOD.

  METHOD create_vendor_data.
    "Local Data Declarations


    DATA: lt_return          TYPE i,
          wa_dfkkbptaxnum    TYPE dfkkbptaxnum,
          it_dfkkbptaxnum    TYPE STANDARD TABLE OF dfkkbptaxnum,
          wa_vbut0id         TYPE but0id,
          it_vbut0id         TYPE STANDARD TABLE OF but0id,
          it_fiwtin_tan_exem TYPE STANDARD TABLE OF fiwtin_tan_exem,
          wa_fiwtin_tan_exem TYPE fiwtin_tan_exem.

    DATA : lv_kz_flag  TYPE c,
           lv_flag_tax TYPE c.

    DATA : ls_wtax    TYPE ty_withtax,
           ls_withtax TYPE ty_withtax,
           ls_xlfbw   TYPE flfbw,
           lt_xlfbw   TYPE TABLE OF flfbw INITIAL SIZE 0,
           lt_ylfbw   TYPE TABLE OF flfbw INITIAL SIZE 0.

    DATA : lt_withtax TYPE TABLE OF ty_withtax INITIAL SIZE 0.


    REFRESH:it_dfkkbptaxnum,it_vbut0id,
    it_fiwtin_tan_exem,
    it_lfa1.
    CLEAR:wa_dfkkbptaxnum,wa_vbut0id,
    lt_return,
    gs_vmds_extern,
    gs_vmds_succ,
    gs_err_messages,
    gs_vmds_error,
    wa_fiwtin_tan_exem.

    lt_return = me->prepare_data_vend( ).

*   Do not proceed if the Vendor Data for creation was not prepared
    IF lt_return IS NOT INITIAL.
      RETURN ."EXIT.
    ENDIF.

    "Initialize all the Data
    CALL METHOD vmd_ei_api=>initialize.

    "Call the Method for Creating Vendor and Company data and Purchasing Data
    CALL METHOD vmd_ei_api=>maintain_bapi
      EXPORTING
*       IV_TEST_RUN              = SPACE
        iv_collect_messages      = 'X'
        is_master_data           = gs_vmds_extern
      IMPORTING
        es_master_data_correct   = gs_vmds_succ
        es_message_correct       = gs_succ_messages
        es_master_data_defective = gs_vmds_error
        es_message_defective     = gs_err_messages.

    IF businesspartner IS NOT INITIAL AND gs_err_messages-is_error IS INITIAL.

      "To Update Vendor GSTN No
      IF wa_vendor-gstn IS NOT INITIAL.
        wa_dfkkbptaxnum-client  = sy-mandt.
        wa_dfkkbptaxnum-partner = businesspartner.
        wa_dfkkbptaxnum-taxtype = 'IN3'.
        wa_dfkkbptaxnum-taxnum  = wa_vendor-gstn.
        APPEND wa_dfkkbptaxnum TO it_dfkkbptaxnum.
        MODIFY dfkkbptaxnum FROM TABLE it_dfkkbptaxnum. "#EC CI_IMUD_NESTED "#EC CI_SUBRC
      ENDIF.
      CLEAR wa_dfkkbptaxnum.
      REFRESH it_dfkkbptaxnum.

      IF wa_vendor-type IS NOT INITIAL.
        wa_vbut0id-client  = sy-mandt.
        wa_vbut0id-partner = businesspartner.
        wa_vbut0id-type = wa_vendor-type.
        wa_vbut0id-idnumber = wa_vendor-idnumber.
        APPEND wa_vbut0id TO it_vbut0id.
        DATA: lv_but0id TYPE BUT0id.
        CLEAR : lv_but0id.
        SELECT SINGLE * FROM but0id
          INTO LV_but0id
          WHERE partner = businesspartner.
        IF sy-subrc = 0.
          IF lv_but0id IS NOT INITIAL.
            DELETE but0id FROM LV_but0id.
          ENDIF.
        ENDIF.
        MODIFY but0id FROM TABLE it_vbut0id. "#EC CI_IMUD_NESTED "#EC CI_SUBRC
      ENDIF.
      CLEAR : wa_vbut0id.
      REFRESH : it_vbut0id.


      "To Update TAN Based Excemption
      LOOP AT lt_vendor INTO lw_vendor WHERE bpartner EQ businesspartner."WA_VENDOR-BPARTNER.
        IF lw_vendor-wt_exnr1 IS NOT INITIAL.
          wa_fiwtin_tan_exem-bukrs           = lw_vendor-bukrs.
          wa_fiwtin_tan_exem-koart           = 'K'.
          wa_fiwtin_tan_exem-accno           = lw_vendor-bpartner.
          wa_fiwtin_tan_exem-fiwtin_exem_thr = lw_vendor-fiwtin_exem_thr.
          wa_fiwtin_tan_exem-seccode         = lw_vendor-seccode.
          wa_fiwtin_tan_exem-witht           = lw_vendor-witht1.
          wa_fiwtin_tan_exem-wt_withcd       = lw_vendor-wt_withcd1.
          wa_fiwtin_tan_exem-wt_exdf         = lw_vendor-wt_exdf1.
          wa_fiwtin_tan_exem-wt_exdt         = lw_vendor-wt_exdt1.
          wa_fiwtin_tan_exem-wt_exnr         = lw_vendor-wt_exnr1.
          wa_fiwtin_tan_exem-wt_exrt         = lw_vendor-wt_exrt1.
          wa_fiwtin_tan_exem-wt_wtexrs       = lw_vendor-wt_wtexrs1.
          wa_fiwtin_tan_exem-waers           = lw_vendor-waers1.

          APPEND wa_fiwtin_tan_exem TO it_fiwtin_tan_exem.
        ENDIF.
        CLEAR wa_fiwtin_tan_exem.
      ENDLOOP.

      IF it_fiwtin_tan_exem[] IS NOT INITIAL.
        MODIFY fiwtin_tan_exem FROM TABLE it_fiwtin_tan_exem. "#EC CI_IMUD_NESTED   "#EC CI_SUBRC
        REFRESH it_fiwtin_tan_exem.

        CALL FUNCTION 'BAPI_TRANSACTION_COMMIT'
          EXPORTING
*           WAIT = 'X'
            wait = 'X'.
      ENDIF.


    ELSE.
      REFRESH it_return.
      it_return[] = gs_err_messages-messages[].
      LOOP AT it_return INTO wa_return WHERE type EQ 'E'.
        PERFORM sub_log_update USING wa_return-message '002' 'E'.
      ENDLOOP.
    ENDIF.

    IF wa_vendor-j_1ipanno IS NOT INITIAL.
      lfa1-j_1ipanno = wa_vendor-j_1ipanno.
      CONDENSE lfa1-j_1ipanno.
    ENDIF.
    IF wa_vendor-ven_class IS NOT INITIAL.
      lfa1-ven_class = wa_vendor-ven_class.
    ENDIF.
    IF wa_vendor-gstn IS NOT INITIAL.
      lfa1-stcd3     = wa_vendor-gstn.
    ENDIF.

    IF lfa1-lifnr IS NOT INITIAL.
      APPEND lfa1 TO it_lfa1.
**      MODIFY LFA1 FROM LFA1 .                      "#EC CI_IMUD_NESTED
      MODIFY lfa1 FROM TABLE it_lfa1.
    ENDIF.

    IF sy-subrc EQ 0.
*      COMMIT WORK AND WAIT.
      COMMIT WORK.

      IF wa_vendor-witht IS NOT INITIAL.
        CLEAR : ls_withtax,lv_kz_flag.

        SELECT lifnr,
              bukrs,
              witht,
              wt_subjct,
              qsrec,
              wt_wtstcd,
              wt_withcd
          FROM lfbw
          INTO TABLE @DATA(lt_lfbw)
          WHERE lifnr = @businesspartner
           AND bukrs = @wa_vendor-bukrs.
        IF sy-subrc = 0.


          SORT : lt_lfbw BY lifnr bukrs witht.
          READ TABLE lt_lfbw  ASSIGNING FIELD-SYMBOL(<fs_lfbw>)  WITH KEY lifnr = businesspartner
                                                                          bukrs = wa_vendor-bukrs
                                                                          witht = wa_vendor-witht BINARY SEARCH.
          IF sy-subrc = 0 .
            IF <fs_lfbw> IS ASSIGNED AND <fs_lfbw> IS NOT INITIAL.
              IF ( wa_vendor-wt_withcd = <fs_lfbw>-wt_withcd AND wa_vendor-qsrec = <fs_lfbw>-qsrec ).

              ELSE.
                ls_withtax-lifnr      = businesspartner.
                ls_withtax-bukrs      = wa_vendor-bukrs.
                ls_withtax-witht      = wa_vendor-witht.
                ls_withtax-wt_withcd  = wa_vendor-wt_withcd.
                ls_withtax-wt_subjct  = wa_vendor-wt_subjct.
                ls_withtax-qsrec      = wa_vendor-qsrec.
                lv_kz_flag           = 'U'.
                APPEND ls_withtax TO lt_withtax.
                CLEAR ls_withtax.
              ENDIF.
            ENDIF.
          ELSE.
            CLEAR : lv_flag_tax.
            lv_flag_tax = 'X'.
*            LS_WITHTAX-LIFNR      = BUSINESSPARTNER.

          ENDIF.
        ELSE.
          CLEAR : lv_flag_tax.
          lv_flag_tax = 'X'.
*          LS_WITHTAX-LIFNR      = BUSINESSPARTNER.

        ENDIF.

        IF  lv_flag_tax = 'X'."LT_WITHTAX IS INITIAL.
          ls_withtax-lifnr      = businesspartner.
          ls_withtax-bukrs      = wa_vendor-bukrs.
          ls_withtax-witht      = wa_vendor-witht.
          ls_withtax-wt_withcd  = wa_vendor-wt_withcd.
          ls_withtax-wt_subjct  = wa_vendor-wt_subjct.
          ls_withtax-qsrec      = wa_vendor-qsrec.
          lv_kz_flag = 'I'.
          APPEND ls_withtax TO lt_withtax.
          CLEAR ls_withtax.
        ENDIF.

        IF lt_withtax IS NOT INITIAL.
          LOOP AT lt_withtax INTO ls_withtax.      "#EC CI_LOOP_INTO_WA
            CLEAR ls_xlfbw.
            ls_xlfbw-lifnr     = ls_withtax-lifnr.
            ls_xlfbw-bukrs     = ls_withtax-bukrs.
            ls_xlfbw-witht     = ls_withtax-witht.
            ls_xlfbw-wt_subjct = ls_withtax-wt_subjct.
            ls_xlfbw-qsrec     = ls_withtax-qsrec.
            ls_xlfbw-wt_withcd = ls_withtax-wt_withcd.
            ls_xlfbw-kz        = lv_kz_flag.
            APPEND ls_xlfbw TO lt_xlfbw.
            CLEAR ls_xlfbw.

            CALL FUNCTION 'FI_WT_VENDOR_UPDATE'
              TABLES
                t_xlfbw = lt_xlfbw
                t_ylfbw = lt_ylfbw.

            REFRESH lt_xlfbw[].
          ENDLOOP.
        ENDIF.
      ENDIF.
***************************************************
    ENDIF.
  ENDMETHOD.

ENDCLASS.
*&---------------------------------------------------------------------*
*& Form SUB_CHANGE_BP
*&---------------------------------------------------------------------*
FORM sub_change_bp .
  businesspartner = wa_vendor-bpartner.

  CALL FUNCTION 'BAPI_BUPA_CENTRAL_CHANGE'
    EXPORTING
      businesspartner           = wa_vendor-bpartner
      centraldata               = centraldata
      centraldataperson         = centraldataperson
      centraldataorganization   = centraldataorganization
      centraldata_x             = centraldatax
      centraldataperson_x       = centraldatapersonx
      centraldataorganization_x = centraldataorganizationx
    TABLES
      return                    = it_return.
*
*  CALL FUNCTION 'BAPI_TRANSACTION_COMMIT'
*    EXPORTING
*      WAIT = ' '.

*  BREAK-POINT.
  "To Change Address
  CALL FUNCTION 'BAPI_BUPA_ADDRESS_CHANGE'
    EXPORTING
      businesspartner = wa_vendor-bpartner
      addressdata     = addressdata
      addressdata_x   = addressdatax
    TABLES
      bapiadtel       = it_telephondata
      bapiadsmtp      = it_maildata
      bapiadtel_x     = it_teleph_datax
      bapiadsmt_x     = it_maildatax
      return          = it_return.


  CALL FUNCTION 'BAPI_TRANSACTION_COMMIT'
    EXPORTING
      wait = ' '.

  CLEAR wa_return.
  READ TABLE it_return INTO wa_return WITH KEY type = 'E'.
  IF sy-subrc EQ 0.
    PERFORM sub_log_update USING wa_return-message '002' 'E'.
  ELSE.
    PERFORM sub_log_update USING 'Business Partner Changed'(022) '001' 'S'.
  ENDIF.
ENDFORM.
*&---------------------------------------------------------------------*
*& Form SUB_BP_ROLE
*&---------------------------------------------------------------------*

FORM sub_bp_role .
  DATA businesspartnerrolecategory TYPE bapibus1006_bproles-partnerrolecategory.
  DATA businesspartnerrole         TYPE bapibus1006_bproles-partnerrole.
  DATA differentiationtypevalue    TYPE bapibus1006_bproles-difftypevalue.

  DATA lt_return                      TYPE STANDARD TABLE OF bapiret2.

  businesspartnerrole = 'FLVN00'.
  CALL FUNCTION 'BAPI_BUPA_ROLE_ADD_2'
    EXPORTING
      businesspartner             = businesspartner
      businesspartnerrolecategory = businesspartnerrolecategory
      all_businesspartnerroles    = ' '
      businesspartnerrole         = businesspartnerrole
      differentiationtypevalue    = differentiationtypevalue
      validfromdate               = sy-datum
      validuntildate              = '99991231'
    TABLES
      return                      = lt_return.
  CALL FUNCTION 'BAPI_TRANSACTION_COMMIT'
    EXPORTING
*     WAIT = 'X'.
      wait = 'X'.

  businesspartnerrole = 'FLVN01'.
  CALL FUNCTION 'BAPI_BUPA_ROLE_ADD_2'
    EXPORTING
      businesspartner             = businesspartner
      businesspartnerrolecategory = businesspartnerrolecategory
      all_businesspartnerroles    = ' '
      businesspartnerrole         = businesspartnerrole
      differentiationtypevalue    = differentiationtypevalue
      validfromdate               = sy-datum
      validuntildate              = '99991231'
    TABLES
      return                      = lt_return.

  CALL FUNCTION 'BAPI_TRANSACTION_COMMIT'
    EXPORTING
*     WAIT = 'X'.
      wait = 'X'.

ENDFORM.
*&---------------------------------------------------------------------*
*& Form SUB_BANK_DETAILS
*&---------------------------------------------------------------------*

FORM sub_bank_details .
  DATA:bbpartner         TYPE bapibus1006_head-bpartner,
       bkvid             TYPE but0bk-bkvid,
       it_retun          TYPE STANDARD TABLE OF bapiret2,
       wa_retun          TYPE  bapiret2,
       lw_vendor         TYPE ty_vend,
       bankdetaildata    TYPE  bapibus1006_bankdetail,
       bankdetaildatax   TYPE  bapibus1006_bankdetail_x,
*       LW_BANKDETAILDATA TYPE STANDARD TABLE OF BAPIBUS1006_BANKDETAIL WITH HEADER LINE,
       lw_bankdetaildata TYPE bapibus1006_bankdetail,
       bnka              TYPE bnka,
       country           TYPE bapi1011_key-bank_ctry,
       key               TYPE bapi1011_key-bank_key,
       address           TYPE bapi1011_address.
*       BP_LOCK           TYPE BUT000-PARTNER.

  CLEAR:bbpartner,bankdetaildata,bkvid,lw_bankdetaildata.
  REFRESH: it_retun.
  lw_vendor = wa_vendor.


  IF lw_vendor-bank_key IS NOT INITIAL AND lw_vendor-bank_acct IS NOT INITIAL.
    bbpartner = lfa1-lifnr. "lw_vendor-bpartner.
    bkvid     = lw_vendor-bkvid.

**
*   BANKDETAILDATA-BANKDETAILMOVEID = LW_VENDOR-BKVID.
*

    bankdetaildata-bank_ctry      = lw_vendor-bank_ctry.
    bankdetaildata-bank_key       = lw_vendor-bank_key.
    bankdetaildata-bank_acct      = lw_vendor-bank_acct.
    bankdetaildata-ctrl_key       = lw_vendor-ctrl_key.
    bankdetaildata-accountholder  = lw_vendor-accountholder.

*    BANKDETAILDATAX-BANKDETAILMOVEID = 'X'.

    bankdetaildatax-bank_ctry       = 'X'.
    bankdetaildatax-bank_key        = 'X'.
    bankdetaildatax-bank_acct       = 'X'.
    bankdetaildatax-ctrl_key        = 'X'.
    bankdetaildatax-accountholder   = 'X'.

    "Checking If the Bank Master was Created or Not
    CLEAR:bnka,wa_retun,country,key,bkvid.
    SELECT SINGLE * FROM bnka INTO bnka WHERE banks EQ lw_vendor-bank_ctry AND bankl EQ lw_vendor-bank_key. "#EC CI_SUBRC
    IF bnka IS INITIAL.
      country = lw_vendor-bank_ctry.
      key = lw_vendor-bank_key.
      address-bank_name = lw_vendor-banka.

      CALL FUNCTION 'BAPI_BANK_CREATE'
        EXPORTING
          bank_ctry    = country
          bank_key     = key
          bank_address = address
          i_xupdate    = 'X'
        IMPORTING
          return       = wa_retun.

      CALL FUNCTION 'BAPI_TRANSACTION_COMMIT'.
    ENDIF.
*    ##WARN_OK      SELECT SINGLE bkvid FROM but0bk INTO bkvid WHERE partner EQ bbpartner AND bankl EQ lw_vendor-bank_key AND bankn EQ lw_vendor-bank_acct. "#EC CI_SUBRC

    bkvid     = lw_vendor-bkvid.

    CALL FUNCTION 'BAPI_BUPA_BANKDETAIL_GETDETAIL'
      EXPORTING
        businesspartner = businesspartner "bbpartner
        bankdetailid    = bkvid " bkvid
      IMPORTING
        bankdetaildata  = lw_bankdetaildata.

*****Wait to update the bank details*****
    WAIT UP TO '0.1' SECONDS.


    IF lw_bankdetaildata IS INITIAL.
      REFRESH it_retun.
      CALL FUNCTION 'BAPI_BUPA_BANKDETAIL_ADD'
        EXPORTING
          businesspartner = businesspartner "bbpartner
          bankdetailid    = wa_vendor-bkvid
          bankdetaildata  = bankdetaildata
        TABLES
          return          = it_retun.
      IF it_retun[] IS INITIAL.
        CALL FUNCTION 'BAPI_TRANSACTION_COMMIT'.
*        PERFORM SUB_LOG_UPDATE USING 'Business Partner Bank Details added' '001' 'S'.
      ELSE.
        READ TABLE it_retun INTO wa_retun WITH KEY type = 'E'.
        IF sy-subrc EQ 0.
          IF wa_retun-id NE 'R1' AND wa_return-number NE '086'.
            PERFORM sub_log_update USING wa_retun-message '002' 'E'.
          ENDIF.
        ELSE.
        ENDIF.
      ENDIF.
    ELSE.

      REFRESH it_retun.
      CALL FUNCTION 'BAPI_BUPA_BANKDETAIL_CHANGE'
        EXPORTING
          businesspartner  = businesspartner  "bbpartner
          bankdetailid     = bkvid
          bankdetaildata   = bankdetaildata
          bankdetaildata_x = bankdetaildatax
        TABLES
          return           = it_retun.

      IF it_retun[] IS INITIAL.
        CALL FUNCTION 'BAPI_TRANSACTION_COMMIT'.
*        PERFORM SUB_LOG_UPDATE USING 'Business Partner Bank Details Changed' '001' 'S'.
      ELSE.
        READ TABLE it_retun INTO wa_retun WITH KEY type = 'E'.
        IF sy-subrc EQ 0.                                 "#EC CI_SUBRC
*           Do nothing
        ELSEIF sy-subrc NE 0.
*           Do nothing.
        ENDIF.
      ENDIF.
    ENDIF.

    CLEAR:bbpartner,bankdetaildata,bkvid,lw_bankdetaildata,bankdetaildatax,lw_bankdetaildata,lw_vendor.
    REFRESH: it_retun.
  ENDIF.
ENDFORM.

*&---------------------------------------------------------------------*
*& Form SUB_VALIDATE_FILE
*&---------------------------------------------------------------------*

FORM sub_validate_file .
  CLEAR:vend.
  IF p_file IS NOT INITIAL.
    IF p_file CS 'VEND'.
      vend = 'X'.
    ELSE.
      MESSAGE 'Please select Vendor File'(027) TYPE 'E'.
    ENDIF.
  ENDIF.
ENDFORM.
*&---------------------------------------------------------------------*
*& Form SUB_INIT
*&---------------------------------------------------------------------*

FORM sub_init .

  REFRESH:gt_vendor,lt_vendor,
  it_telephondata,
  it_faxdata,it_maildata,
  it_return.

  CLEAR:vend,
        wa_return,
        wa_maildata,
        wa_faxdata,
        wa_telephondata,
        addressdata,
        addressdatax,
        centraldataorganizationx,
        centraldataorganization,
        centraldatapersonx,
        centraldataperson,
        centraldatax,
        centraldata,
        partnergroup,
        partnercategory,
        businesspartnerextern,
        businesspartner.

ENDFORM.
*&---------------------------------------------------------------------*
*& Form SUB_DISP_LOG
*&---------------------------------------------------------------------*
FORM sub_disp_log .
  wa_layout-colwidth_optimize = 'X'.
  wa_layout-zebra = 'X'.

  wa_fct-fieldname = 'MSGTYP'.
  wa_fct-seltext_l = 'Message Type'(018).
  APPEND wa_fct TO it_fct.
  CLEAR wa_fct.

  wa_fct-fieldname = 'MSGNR'.
  wa_fct-seltext_l = 'Message No'(019).
  APPEND wa_fct TO it_fct.
  CLEAR wa_fct.

  wa_fct-fieldname = 'BPNAME'.
  wa_fct-no_zero = 'X'.
  wa_fct-seltext_l = 'Serial Number'(020).
  APPEND wa_fct TO it_fct.
  CLEAR wa_fct.

  wa_fct-fieldname = 'BP_LIFNR'.
  wa_fct-no_zero = 'X'.
  wa_fct-seltext_l = 'Business Partner'(021).
  APPEND wa_fct TO it_fct.
  CLEAR wa_fct.

  wa_fct-fieldname = 'MSG'.
  wa_fct-seltext_l = 'Message'(002).
  APPEND wa_fct TO it_fct.

  SORT it_log BY bpname.

  CALL FUNCTION 'REUSE_ALV_GRID_DISPLAY'
    EXPORTING
      i_grid_title  = 'Log'
      is_layout     = wa_layout
      it_fieldcat   = it_fct
    TABLES
      t_outtab      = it_log
    EXCEPTIONS
      program_error = 1
      OTHERS        = 2.
  IF sy-subrc <> 0.                                       "#EC CI_SUBRC
* Implement suitable error handling here
  ELSEIF sy-subrc = 0.
  ENDIF.
ENDFORM.
*&---------------------------------------------------------------------*
*& Form SUB_INIT1
*&---------------------------------------------------------------------*
FORM sub_init_vend .
  REFRESH:it_telephondata,it_faxdata,it_maildata,it_return.

  CLEAR:wa_return,wa_maildata,wa_faxdata,wa_telephondata,addressdata,addressdatax,
        centraldataorganizationx,centraldataorganization,centraldatapersonx,
        centraldataperson,centraldatax,centraldata,partnergroup,partnercategory,
        businesspartnerextern,businesspartner,lfa1.

ENDFORM.
*&---------------------------------------------------------------------*
*& Form SUB_LOG_E_UPDATE
*&---------------------------------------------------------------------*
FORM sub_log_update  USING message no typ.
  wa_log-bp_lifnr = businesspartner.
  wa_log-bpname   = bpname.
  wa_log-uzeit    = sy-uzeit.
  wa_log-erdat    = sy-datum.
  wa_log-uname    = sy-uname.
  wa_log-msg      = message.
  wa_log-msgnr    = no.
  wa_log-msgtyp   = typ.
  APPEND wa_log TO it_log.
  CLEAR wa_log.
ENDFORM.


*&---------------------------------------------------------------------*
*& Form SUB_CREATE_BP_INT
*&---------------------------------------------------------------------*
*& text
*&---------------------------------------------------------------------*

FORM sub_create_bp_int .
  CLEAR businesspartner.
  businesspartnerextern = wa_vendor-bpartner.
  CALL FUNCTION 'BAPI_BUPA_CREATE_FROM_DATA'
    EXPORTING
      businesspartnerextern   = businesspartnerextern
      partnercategory         = partnercategory
      partnergroup            = partnergroup
      centraldata             = centraldata
      centraldataperson       = centraldataperson
      centraldataorganization = centraldataorganization
      addressdata             = addressdata
    IMPORTING
      businesspartner         = businesspartner
    TABLES
      telefondata             = it_telephondata
      faxdata                 = it_faxdata
      e_maildata              = it_maildata
      return                  = it_return.

  CLEAR wa_return.
  READ TABLE it_return INTO wa_return WITH KEY type = 'E'.
  IF sy-subrc EQ 0.
    PERFORM sub_log_update USING wa_return-message '002' 'E'.
  ELSE.
    PERFORM sub_log_update USING 'Business Partner Created'(014) '001' 'S'.
  ENDIF.

  IF businesspartner IS NOT INITIAL.
    CALL FUNCTION 'BAPI_TRANSACTION_COMMIT'
      EXPORTING
        wait = 'X'.

*    LOOP AT gt_vendor INTO lw_vendor WHERE index EQ wa_vendor-index.
      lw_vendor-bpartner = businesspartner.
      wa_vendor-bpartner = businesspartner.
      MODIFY gt_vendor FROM lw_vendor TRANSPORTING bpartner.
      CLEAR lw_vendor.
*    ENDLOOP.

    LOOP AT lt_vendor INTO lw_vendor WHERE index = wa_vendor-index.
      lw_vendor-bpartner = businesspartner.
      MODIFY lt_vendor FROM lw_vendor TRANSPORTING bpartner.
      CLEAR lw_vendor.
    ENDLOOP.
  ENDIF.
ENDFORM.
