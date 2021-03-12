# utl-use-the-variable-names-in-table2-to-rename-table1
Use the variable names in table2 to rename table1

    Use the variable names in table2 to rename table1 in R and SAS

       Two Solutions
          a. sas
          b. R  (code is similar both use array of names )

    GitHub
    https://tinyurl.com/s5bs9hf5
    https://github.com/rogerjdeangelis/utl-use-the-variable-names-in-table2-to-rename-table1

    macros
    https://tinyurl.com/y9nfugth
    https://github.com/rogerjdeangelis/utl-macros-used-in-many-of-rogerjdeangelis-repositories

    Inspired by
    https://tinyurl.com/2n3vm3n6
    https://stackoverflow.com/questions/66012439/renaming-all-variables-from-a-sas-table

    *_                   _
    (_)_ __  _ __  _   _| |_
    | | '_ \| '_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    ;

    libname sd1 "d:/sd1";

    data sd1.old;

      alpha =1;
      beta  =2;
      gamma =3;

    run;quit;

    /*
     Variables in Creation Order

    #    Variable    Type    Len

    1    ALPHA       Num       8
    2    BETA        Num       8
    3    GAMMA       Num       8
    */

    data sd1.new;

      a =1;
      b =2;
      c =3;

    run;quit;

    /*
     Variables in Creation Order

    #    Variable    Type    Len

    1    ALPHA       Num       8
    2    BETA        Num       8
    3    GAMMA       Num       8
    */

    *
     _ __  _ __ ___   ___ ___  ___ ___
    | '_ \| '__/ _ \ / __/ _ \/ __/ __|
    | |_) | | | (_) | (_|  __/\__ \__ \
    | .__/|_|  \___/ \___\___||___/___/
    |_|
      __ _     ___  __ _ ___
     / _` |   / __|/ _` / __|
    | (_| |_  \__ \ (_| \__ \
     \__,_(_) |___/\__,_|___/

    ;

    %array(old,values=%varlist(sd1.old));
    %array(new,values=%varlist(sd1.new));

    proc datasets lib=sd1;
      modify old;
        rename
        %do_over(old new,phrase=%str(
           ?old = ?new
        ));
    run;quit;

    *_        ____
    | |__    |  _ \
    | '_ \   | |_) |
    | |_) |  |  _ <
    |_.__(_) |_| \_\

    ;

    %utl_submit_r64('
    library(haven);
    library(SASxport);
    old=read_sas("d:/sd1/old.sas7bdat");
    new=read_sas("d:/sd1/new.sas7bdat");
    colnames(old)=colnames(new);
    write.xport(old,file="d:/xpt/old.xpt");
    ');

    libname xpt xport "d:/xpt/old.xpt";
    proc contents data=xpt.old;
    run;quit;

    *            _         _
      ___  _   _| |_ _ __ | |_
     / _ \| | | | __| '_ \| __|
    | (_) | |_| | |_| |_) | |_
     \___/ \__,_|\__| .__/ \__|
                    |_|
    ;

    OLD Table now has the new alphabet

    Data Set Name  WORK.OLD  Observations          1
    Member Type    DATA      Variables             3

     Variables in Creation Order

    #    Variable    Type    Len

    1    A           Num       8
    2    B           Num       8
    3    C           Num       8




