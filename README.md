# utl-select-all-rows-and-add-row-with-sum-of-two-specific-rows-sql-sas-r-and-python-multi-language
Select all rows and add row with sum of two specific rows sql sas r and python multi-language
    %let pgm=utl-select-all-rows-and-add-row-with-sum-of-two-specific-rows-sql-sas-r-and-python-multi-language;

    Select all rows and add row with sum of two specific rows sql sas r and python multi-language

    github
    https://tinyurl.com/34vy2v9m
    https://github.com/rogerjdeangelis/utl-select-all-rows-and-add-row-with-sum-of-two-specific-rows-sql-sas-r-and-python-multi-language

    stackoverflow R
    https://tinyurl.com/4rfwnd4t
    https://stackoverflow.com/questions/79222370/how-to-add-two-rows-to-get-an-additional-row

       SOLUTIONS

             1 SAS SQL
             2 R SQL
             3 PYTHON SQL
             4 R Not Run see link

    /*               _     _
     _ __  _ __ ___ | |__ | | ___ _ __ ___
    | `_ \| `__/ _ \| `_ \| |/ _ \ `_ ` _ \
    | |_) | | | (_) | |_) | |  __/ | | | | |
    | .__/|_|  \___/|_.__/|_|\___|_| |_| |_|
    |_|
    */

    /**************************************************************************************************************************/
    /*              |                                                        |                                                */
    /*     INPUT    |                PROCESS                                 |      OUTPUT                                    */
    /*   =========  |    =========================================           |   =============                                */
    /*              |                                                        |                                                */
    /*   X    H12A  |    SUM COLUMN H12A WHEN  X=1 or x=2                    |     X     H12A                                 */
    /*              |    AND ADD ROW ON END OF INPUT                         |                                                */
    /*   1     0.5  |                                                        |    1.0     0.5                                 */
    /*   2     0.1  |    X   H12A          X    H12A                         |    2.0     0.1                                 */
    /*   3     0.1  |                                                        |    3.0     0.1                                 */
    /*   4     0.3  |    1    0.5          1     0.5                         |    4.0     0.3                                 */
    /*              |    2    0.1  SUM=.6  2     0.1                         |    1.2     0.6  Added                          */
    /*              |                      3     0.1                         |                                                */
    /*              |    3    0.1          4     0.3                         |                                                */
    /*              |    4    0.3                                            |                                                */
    /*              |                      1.2   0.6 ADDED X=1 or X=2        |                                                */
    /*              |                                                        |                                                */
    /*              | -------------------------------------------------------|                                                */
    /*              |                                                        |                                                */
    /*              |  SAS R AND PYTHON (NO KLINGON CODE)                    |                                                */
    /*              |                                                        |                                                */
    /*              |    EXACT SAME CODE IN SAS R AND PYTHON                 |                                                */
    /*              |    SELF EXPLANATORY                                    |                                                */
    /*              |                                                        |                                                */
    /*              |      select                                            |                                                */
    /*              |         x                                              |                                                */
    /*              |        ,h12a                                           |                                                */
    /*              |      from                                              |                                                */
    /*              |         sd1.have                                       |                                                */
    /*              |      union                                             |                                                */
    /*              |         all                                            |                                                */
    /*              |      select                                            |                                                */
    /*              |         1.2 as x                                       |                                                */
    /*              |        ,sum(h12a) as h12a                              |                                                */
    /*              |      from                                              |                                                */
    /*              |         sd1.have                                       |                                                */
    /*              |      where                                             |                                                */
    /*              |         x=1 or x=2                                     |                                                */
    /*              |      group                                             |                                                */
    /*              |        by 1                                            |                                                */
    /*              |                                                        |                                                */
    /*              |--------------------------------------------------------|                                                */
    /*              |                                                        |                                                */
    /*              |  R  tidyverse language ('<- \(','}}))','mutate' '|>')  |                                                */
    /*              |                                                        |                                                */
    /*              |     R SOLUTION  (NOT REPRODUCED see LINK)              |                                                */
    /*              |                                                        |                                                */
    /*              |     add_row_custom <- \(data.frame, column             |                                                */
    /*              |      ,levels, question) {                              |                                                */
    /*              |       new_row <- data.frame |>                         |                                                */
    /*              |        filter({{column}} %in% levels) |>               |                                                */
    /*              |        summarise(                                      |                                                */
    /*              |            {{column}} := paste(levels, collapse = "_"),|                                                */
    /*              |            {{question}} := sum({{question}})           |                                                */
    /*              |        ) |>                                            |                                                */
    /*              |        mutate({{column}} := as.character({{column}}))  |                                                */
    /*              |       bind_rows(mutate(data.frame, {{column}}          |                                                */
    /*              |       := as.character({{column}})), new_row)           |                                                */
    /*              |                                                        |                                                */
    /*              |     add_row_custom(h12, x, 1:2, h12a)                  |                                                */
    /*              |                                                        |                                                */
    /*              |                                                        |                                                */
    /**************************************************************************************************************************/


    options validvarname=upcase;
    libname sd1 "d:/sd1";

    data sd1.have;
     input x h12a ;
    cards4;
    1 0.5
    2 0.1
    3 0.1
    4 0.3
    ;;;;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  X    H12A                                                                                                             */
    /*                                                                                                                        */
    /*  1     0.5                                                                                                             */
    /*  2     0.1                                                                                                             */
    /*  3     0.1                                                                                                             */
    /*  4     0.3                                                                                                             */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*                             _
    / |  ___  __ _ ___   ___  __ _| |
    | | / __|/ _` / __| / __|/ _` | |
    | | \__ \ (_| \__ \ \__ \ (_| | |
    |_| |___/\__,_|___/ |___/\__, |_|
                                |_|
    */


    proc sql;
      create
         table want as
      select
         x
        ,h12a
      from
         sd1.have
      union
         all
      select
         1.2 as x
        ,sum(h12a) as h12a
      from
         sd1.have
      where
         x=1 or x=2
      group
         by 1
    ;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*     INPUT                                                                                                              */
    /*   =========                                                                                                            */
    /*                                                                                                                        */
    /*   X    H12A                                                                                                            */
    /*                                                                                                                        */
    /*   1     0.5                                                                                                            */
    /*   2     0.1                                                                                                            */
    /*   3     0.1                                                                                                            */
    /*   4     0.3                                                                                                            */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*___                     _
    |___ \   _ __   ___  __ _| |
      __) | | `__| / __|/ _` | |
     / __/  | |    \__ \ (_| | |
    |_____| |_|    |___/\__, |_|
                           |_|
    */

    proc datasets lib=sd1 nolist nodetails;
     delete want;
    run;quit;

    %utl_rbeginx;
    parmcards4;
    library(haven)
    library(sqldf)
    source("c:/oto/fn_tosas9x.R")
    have<-read_sas("d:/sd1/have.sas7bdat")
    have
    want<-sqldf('
      select
        x
       ,h12a
      from
        have
      union
        all
      select
        1.2 as x
       ,sum(h12a) as h12a
      from
        have
      where
        x=1 or x=2
      group
        by 1
      ')
    want
    fn_tosas9x(
          inp    = want
         ,outlib ="d:/sd1/"
         ,outdsn ="want"
         )
    ;;;;
    %utl_rendx;

    proc print data=sd1.want;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  R             SAS                                                                                                     */
    /*                                                                                                                        */
    /*      X H12A    ROWNAMES     X     H12A                                                                                 */
    /*                                                                                                                        */
    /*  1 1.0  0.5        1       1.0     0.5                                                                                 */
    /*  2 2.0  0.1        2       2.0     0.1                                                                                 */
    /*  3 3.0  0.1        3       3.0     0.1                                                                                 */
    /*  4 4.0  0.3        4       4.0     0.3                                                                                 */
    /*  5 1.2  0.6        5       1.2     0.6                                                                                 */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*____               _   _                             _
    |___ /   _ __  _   _| |_| |__   ___  _ __    ___  __ _| |
      |_ \  | `_ \| | | | __| `_ \ / _ \| `_ \  / __|/ _` | |
     ___) | | |_) | |_| | |_| | | | (_) | | | | \__ \ (_| | |
    |____/  | .__/ \__, |\__|_| |_|\___/|_| |_| |___/\__, |_|
            |_|    |___/                                |_|
    */

    proc datasets lib=sd1 nolist nodetails;
     delete pywant;
    run;quit;

    %utl_pybeginx;
    parmcards4;
    exec(open('c:/oto/fn_python.py').read());
    have,meta = ps.read_sas7bdat('d:/sd1/have.sas7bdat');
    want=pdsql('''
      select
        x
       ,h12a
      from
        have
      union
        all
      select
        1.2 as x
       ,sum(h12a) as h12a
      from
        have
      where
        x=1 or x=2
      group
        by 1
       ''');
    print(want);
    fn_tosas9x(want,outlib='d:/sd1/',outdsn='pywant',timeest=3);
    ;;;;
    %utl_pyendx;

    proc print data=sd1.pywant;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* PYTHON             SAS                                                                                                 */
    /*                                                                                                                        */
    /*      X  H12A        X     H12A                                                                                         */
    /*                                                                                                                        */
    /* 0  1.0   0.5       1.0     0.5                                                                                         */
    /* 1  2.0   0.1       2.0     0.1                                                                                         */
    /* 2  3.0   0.1       3.0     0.1                                                                                         */
    /* 3  4.0   0.3       4.0     0.3                                                                                         */
    /* 4  1.2   0.6       1.2     0.6                                                                                         */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
