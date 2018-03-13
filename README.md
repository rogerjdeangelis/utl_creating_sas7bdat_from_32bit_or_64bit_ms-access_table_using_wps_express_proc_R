# utl_creating_sas7bdat_from_32bit_or_64bit_ms-access_table_using_wps_express_proc_R
Creating a sas7bdat from 32bit or 64bit MS-Access table using WPS Express proc R.  Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.
    Creating a sas7bdat from 32bit or 64bit MS-Access table using WPS Express proc R

    Original Topic: Import csv or Access table from Window environment to Unix Environment
    SAS7BDAT is binary compatible in Unix and Windows

    see
    github
    https://tinyurl.com/y8f5yczs
    https://github.com/rogerjdeangelis/utl_creating_sas7bdat_from_32bit_or_64bit_ms-access_table_using_wps_express_proc_R

    SAS Forum
    https://tinyurl.com/ybc36peg
    https://communities.sas.com/t5/Base-SAS-Programming/import-csv-or-Access-table-from-Window-environment-to-Unix/m-p/445123

    If you have R and WPS Express on your desktop you can do the following below. Free.

    The full Base version of WPS supports MS Access import/export.
    You can also create an V5 or V8 transport file in R without WPS Xpress.
    This is better than a csv because it provides type and length.

    If you have the IML interface to R you should be able
    to cut and paste the code below.

    I believe the MS Access driver is both 32 and 64 bit (mdb or accdb)


    HAVE (table customer in MS Access 32bit/ or 64bit database, d:/mdb/class.accdb)
    =====================================================================

    The WPS System

    MS ACCESS TABLE Customers

       Customer ID State Zip Code              Country        Phone
    1     14324742    CA    95123                  USA 408/629-0589
    2     14569877    NC    27514                  USA 919/489-6792
    3     14898029    MD    20850                  USA 301/760-2541
    4     15432147    MI    49001                  USA 616/582-3906
    5     18543489    TX    78701                  USA 512/478-0788
    6     19783482    VA    22090                  USA 703/714-2900
    7     19876078    CA    93274                  USA 209/686-3953
    8     26422096  <NA>    75014               France   4268-54-72
    9     26984578  <NA>     5110              Austria     43-57-04
    10    27654351  <NA>     5010              Belgium 02/215-37-32
    11    28710427    HV     3607          Netherlands  (021)570517
    12    29834248  <NA>       NA              Britain (0552)715311
    13    31548901    BC       NA               Canada 406/422-3413
    14    38763919  <NA>     1405            Argentina     244-6324
    15    39045213    SP     1051               Brazil 012/302-1021
    16    43290587  <NA>       NA                Japan (02)933-3212
    17    43459747  <NA>     3181            Australia  03/734-5111
    18    46543295  <NA>       NA                Japan (03)022-2332
    19    46783280  <NA>     2374            Singapore      3762855



    WANT (SAS dataset)
    ==================

    Up to 40 obs from customers total obs=20


           CUSTOMER_
    Obs        ID       STATE    ZIP_CODE    COUNTRY                 PHONE

      1     14324742     CA        95123     USA                     408/629-0589
      2     14569877     NC        27514     USA                     919/489-6792
      3     14898029     MD        20850     USA                     301/760-2541
      4     15432147     MI        49001     USA                     616/582-3906
      5     18543489     TX        78701     USA                     512/478-0788
      6     19783482     VA        22090     USA                     703/714-2900
      7     19876078     CA        93274     USA                     209/686-3953
      8     26422096               75014     France                  4268-54-72
      9     26984578                5110     Austria                 43-57-04
     10     27654351                5010     Belgium                 02/215-37-32
     11     28710427     HV         3607     Netherlands             (021)570517
     12     29834248                   .     Britain                 (0552)715311
     13     31548901     BC            .     Canada                  406/422-3413
     14     38763919                1405     Argentina               244-6324
     15     39045213     SP         1051     Brazil                  012/302-1021
     16     43290587                   .     Japan                   (02)933-3212
     17     43459747                3181     Australia               03/734-5111
     18     46543295                   .     Japan                   (03)022-2332
     19     46783280                2374     Singapore               3762855
     20     48345514                   .     United Arab Emirates    213445


    WORKING CODE

        MS 32bit/64bit free win7  driver

        myDB<-odbcDriverConnect("Driver={Microsoft Access Driver (*.mdb, *.accdb)};DBQ=d:/mdb/class.accdb");
        customers<-sqlQuery(myDB, paste("select * from customers"));

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;

    * two ways to fetch depending on volume;
    %utl_submit_wps64('
    options set=R_HOME "C:/Program Files/R/R-3.3.2";
    libname wrk "%sysfunc(pathname(work))";
    proc r;
    submit;
    library(RODBC);
    myDB<-odbcDriverConnect("Driver={Microsoft Access Driver (*.mdb, *.accdb)};DBQ=d:/mdb/class.accdb");
    customers<-sqlQuery(myDB, paste("select * from customers"));
    customers;
    close(myDB);
    endsubmit;
    import r=customers data=wrk.customers;
    run;quit;
    ');

    /*
    this might be faster for larger tables
    customers<-sqlFetch(myDB, "customers", max = 20, rows_at_time = 10);
    */


    NOTE: Using R version 3.3.2 (2016-10-31) from C:/Program Files/R/R-3.3.2

    NOTE: Processing of R statements complete

    13        import r=customers data=wrk.customers;
    NOTE: Creating data set 'WRK.customers' from R data frame 'customers'
    NOTE: Column names modified during import of 'customers'
    NOTE: Data set "WRK.customers" has 20 observation(s) and 10 variable(s)




