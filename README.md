# jnr-jenner-analytics-better-proc-contents-for-libraries-of-mixed-data-formats-parquet-avro-sas7bdat
Jenner Analytics a better proc contents for libraries of mixed data formats parquet avro sas7bdat
    Jenner Analytics a better proc contents for libraries of mixed data formats parquet avro sas7bdat

    I expect this will be fixed.

    CONTENTS

      1  PROBLEM
      2  INPUT
      3  SOLUTION
      4  TWO MACROS
           contentsx
           utl_dirContents

    Better 'proc contents"' Displays the data format
    USAGE

      %contentsx(libname=jenhelp,member=iris);

      Builtin proc contents has
      -------------------------
      Data Set Name: IRIS

      Better Proc contents has
      ------------------------
      Data Set Name: C:\Users\roger\AppData\Roaming\jenner\jenhelp\iris.parquet  /*--- DEFAULT LOCATION ---*/
      Observations:  150
      Variables:     5

      Variables (in alphabetical order):

        #  Variable     Type    Len  Format  Label
      ---  -----------  ----  -----  ------  -----------------
        3  PetalLength   Num      8          Petal length (cm)
        4  PetalWidth    Num      8          Petal width (cm)
      ...

    Related repos
    see github
    https://goo.gl/gChmFX
    https://github.com/rogerjdeangelis/utl_file_and_directory_utilities_for_all_operating_systems

    michael.goulding@experis.com
    https://www.pharmasug.org/proceedings/2015/QT/PharmaSUG-2015-QT23.pdf

    /******************************************************************************************************************/
    /*  1 PROBLEM:                                                                                                    */
    /******************************************************************************************************************/

     Jenner proc contents does not list the data format for libraries
     with mutiple table formats, like parquet, sas7bdat and avro.

    CONSIDER a proc contents for a library with parquet, sas7bdat and avro tables
      ie
        CONTENTS    MISSING
        OUTPUT      INFORMATION
        work.iris   (paruet format)   /*--- DOES NOT IDENTIFY THE TABLE FORMAT ---*/
        work.class  (sas7bdat format) /*--- DOES NOT IDENTIFY THE TABLE FORMAT ---*/
        work.cars   (avro format)     /*--- DOES NOT IDENTIFY THE TABLE FORMAT ---*/

        Proc contents data=workx._all_ position;
        run;

        Contents does not provide the table format cards should be avro.

        --- Dataset: CARS --- /*--- DOES NOT IDENTIFY THE TABLE FORMAT SHOULD HAVE AVRO SOMEWHERE---*/
        **** TABLE FORMAT: AVRO MISSING***
        Observations: 428
        Variables: 15

          #  Variable              Type    Len
        ---  --------------------  ----  -----
          1  Make                  Char     13
          2  Model                 Char     11
          3  Type                  Char      8
          ...

        LIKE WISE
        IRIS SHOULD BE PARQUET
        CLASS SHOULD BE SAS&BDAT

    /******************************************************************************************************************/
    /*  2 INPUT                                                                                                       */
    /******************************************************************************************************************/

    /*--- CREATE LIBRARY WITH SAS7BDAt, PARQUET AND AVRO TABLES ---*/

    %let pth=%sysfunc(pathname(workx));

    libname workx "&pth";

    proc datasets lib=workx kill;
    run;

    libname sas7bdat sas7bdat "&pth";
    libname parquet           "&pth";
    libname avro      avro    "&pth";

    data sas7bdat.class;set  jenhelp.class;run;  /*--- interoperable    ---*/
    data parquet.iris;  set  jenhelp.iris;run;   /*--- varchar & bigint ---*/
    data avro.cars;     set  jenhelp.cars;run;   /*--- varchar & bigint ---*/

    =================================================================================================================
    LOG
    =================================================================================================================

    NOTE: Copyright (c) 2026 Jenner Analytics Ltd., London, England.
    NOTE: Jenner v1.5.110 (build v1.5.110+13530c7ccc.20261030T030012Z)
          Licensed to Roger DeAngelis, Serial 3BF7B3E6.
    NOTE: DATA _null_

    autexec started.

    NOTE: DATA elapsed:
      wall  0.00 seconds
      cpu   0.00 seconds
    NOTE: Library WORKX assigned path=d:\wpswrkx.
    NOTE: DATA workx.class

    NOTE: Processing inline DATALINES (19 lines)

    NOTE: Read 19 rows from DATALINES.
    NOTE: The data set WORKX.CLASS has 19 observations and 5 variables.
    NOTE: DATA elapsed:
      wall  0.00 seconds
      cpu   0.00 seconds
    NOTE: DATA _null_

    LOG:    01NOV2026:15:29:29
    NOTE: DATA _null_ completed. Output written to FILE PRINT
    NOTE: Option SASAUTOS changed to c:/otojnr.
    NOTE: DATA _null_

    NOTE: Reading from fileref c:/jnr/runsas_selection.sas (c:/jnr/runsas_selection.sas)
        1 %let pth=%sysfunc(pathname(workx));
        2
        3 libname workx "&pth";
        4
        5 proc datasets lib=workx kill;
        6 run;
        7
        8 libname sas7bdat sas7bdat "&pth";
        9 libname parquet           "&pth";
       10 libname avro      avro    "&pth";
       11
       12 data sas7bdat.class;set  jenhelp.class;run;  /*--- interoperable    ---*/
       13 data parquet.iris;  set  jenhelp.iris;run;   /*--- varchar & bigint ---*/
       14 data avro.cars;     set  jenhelp.cars;run;   /*--- varchar & bigint ---*/

    NOTE: Read 14 rows from c:/jnr/runsas_selection.sas.
    NOTE: DATA elapsed:
      wall  0.00 seconds
      cpu   0.00 seconds
    NOTE: DATA _null_

    autoexec completed    01NOV2026:15:29:29

    NOTE: DATA elapsed:
      wall  0.00 seconds
      cpu   0.00 seconds
    NOTE: Library WORKX assigned path=d:\wpswrkx.
    NOTE: PROC DATASETS library=WORKX
    NOTE:
                           Directory
           Libref             WORKX
           Engine             V9
           Physical Name      d:\wpswrkx
           #    Name                             Member Type
           ----------------------------------------------------
           1    CARS                             DATA
           2    CLASS                            DATA
           3    IRIS                             DATA

    NOTE: KILL option deleted 4 member(s) from library WORKX.
    NOTE: Library SAS7BDAT assigned path=d:\wpswrkx.
    NOTE: Library PARQUET assigned path=d:\wpswrkx.
    NOTE: Library AVRO assigned path=d:\wpswrkx.
    NOTE: DATA sas7bdat.class

    NOTE: Read 19 rows from jenhelp.class.
    NOTE: The data set SAS7BDAT.CLASS has 19 observations and 5 variables.
    NOTE: DATA elapsed:
      wall  0.00 seconds
      cpu   0.02 seconds
    NOTE: DATA parquet.iris

    NOTE: Read 150 rows from jenhelp.iris.
    NOTE: The data set PARQUET.IRIS has 150 observations and 5 variables.
    NOTE: DATA elapsed:
      wall  0.00 seconds
      cpu   0.02 seconds
    NOTE: SASHELP.CARS is a stand-in: its values are generated, not the documented 2004 model-year data.
    NOTE: DATA avro.cars

    NOTE: Read 428 rows from jenhelp.cars.
    NOTE: The data set AVRO.CARS has 428 observations and 15 variables.
    NOTE: DATA elapsed:
      wall  0.01 seconds
      cpu   0.02 seconds

    /******************************************************************************************************************/
    /*  3 PROCESS                                                                                                     */
    /******************************************************************************************************************/

    %macro contentsx(libname=workx,member=all)
       /des="proc contents for libraries of mixed data formats parquet avro sas7bdat";

       %local pathname;

       %let pathname=%qsysfunc(pathname(&libname));
       %utl_dirContents(dir=&pathname,dsout=__want__);

       proc print data=__want__;
       run;

       %if %upcase(&member=ALL) %then %do;

       data _null_;
         set __want__(keep=pathname basefile);
         call execute(cats("proc contents data='",pathname,"';run;"));
       run;
       %end;
       %else %do;
       data _null_;
         set __want__(keep=pathname basefile where=(upcase("&member") =: upcase(basefile)));
         call execute(cats("proc contents data='",pathname,"';run;"));

       %end;

    run;

    %mend contentsx;

    %contentsx(libname=workx,member=iris);

    %contentsx()

    =================================================================================================================
    OUTPUT
    =================================================================================================================


    PROC CONTENTS
    -------------------------------------------------------------------------------

    Data Set Name: d:\wpswrkx\iris.parquet  /*--- PROVIDES DATA FORMAT ---*/
    Observations:  150
    Variables:     5

    Variables (in alphabetical order):

      #  Variable     Type    Len  Format  Label
    ---  -----------  ----  -----  ------  -----------------
      3  PetalLength   Num      8          Petal length (cm)
      4  PetalWidth    Num      8          Petal width (cm)
      1  SepalLength   Num      8          Sepal length (cm)
      2  SepalWidth    Num      8          Sepal width (cm)
      5  Species      Char     10          Iris species

    -------------------------------------------------------------------------------

    =================================================================================================================
    LOG
    =================================================================================================================

    NOTE: Copyright (c) 2026 Jenner Analytics Ltd., London, England.
    NOTE: Jenner v1.5.110 (build v1.5.110+13530c7ccc.20261030T030012Z)
          Licensed to Roger DeAngelis, Serial 3BF7B3E6.
    NOTE: DATA _null_

    autexec started.

    NOTE: DATA elapsed:
      wall  0.00 seconds
      cpu   0.00 seconds
    NOTE: Library WORKX assigned path=d:\wpswrkx.
    NOTE: DATA workx.class

    NOTE: Processing inline DATALINES (19 lines)

    NOTE: Read 19 rows from DATALINES.
    NOTE: The data set WORKX.CLASS has 19 observations and 5 variables.
    NOTE: DATA elapsed:
      wall  0.00 seconds
      cpu   0.00 seconds
    NOTE: DATA _null_

    LOG:    02NOV2026:06:27:57
    NOTE: DATA _null_ completed. Output written to FILE PRINT
    NOTE: Option SASAUTOS changed to c:/otojnr.
    NOTE: DATA _null_

    NOTE: Reading from fileref c:/jnr/runsas_selection.sas (c:/jnr/runsas_selection.sas)
        1 %macro contentsx(libname=workx,member=all)
        2    /des="proc contents for libraries of mixed data formats parquet avro sas7bdat";
        3
        4    %local pathname;
        5
        6    %let pathname=%qsysfunc(pathname(workx));
        7    %utl_dirContents(dir=&pathname,dsout=__want__);
        8
        9    proc print data=__want__;
       10    run;
       11
       12    %if %upcase(&member=ALL) %then %do;
       13
       14    data _null_;
       15      set __want__(keep=pathname basefile);
       16      call execute(cats("proc contents data='",pathname,"';run;"));
       17    run;
       18    %end;
       19    %else %do;
       20    data _null_;
       21      set __want__(keep=pathname basefile where=(upcase("&member") =: upcase(basefile)));
       22      call execute(cats("proc contents data='",pathname,"';run;"));
       23
       24    %end;
       25
       26 run;
       27
       28 %mend contentsx;
       29
       30 %contentsx(libname=workx,member=iris);

    NOTE: Read 30 rows from c:/jnr/runsas_selection.sas.
    NOTE: DATA elapsed:
      wall  0.00 seconds
      cpu   0.00 seconds
    NOTE: DATA _null_

    autoexec completed    02NOV2026:06:27:57

    NOTE: DATA elapsed:
      wall  0.00 seconds
      cpu   0.00 seconds
    NOTE: DATA work.__want__


    NOTE: The data set WORK.__WANT__ has 5 observations and 3 variables.
    NOTE: DATA elapsed:
      wall  0.00 seconds
      cpu   0.00 seconds
    NOTE: DATA work._file_attr


    NOTE: Read 5 rows from __want__.
    NOTE: The data set WORK._FILE_ATTR has 20 observations and 4 variables.
    NOTE: DATA elapsed:
      wall  0.04 seconds
      cpu   0.00 seconds
    NOTE: PROC TRANSPOSE data=_file_attr

    NOTE: Created 5 rows.
    NOTE: Output written to D:\wpswrk\2b102d9d-e8c2-4ec9-9ef4-8909bc74bf50/trans_attr.parquet.
    NOTE: PROC SQL

    NOTE: Macro variable _ATTRIB_VARS = RECFM, LRECL, File_Size__bytes_, Last_Modified
    NOTE: PROC SQL statement used.
    NOTE: DATA work.__want__


    NOTE: The data set WORK.__WANT__ has 5 observations and 7 variables.
    NOTE: DATA elapsed:
      wall  0.03 seconds
      cpu   0.03 seconds
    NOTE: PROC DATASETS library=WORK

    NOTE: Deleting _FILE_ATTR (memtype=DATA).
    NOTE: Deleting TRANS_ATTR (memtype=DATA).
    NOTE: 2 dataset(s) deleted.
    NOTE: __want__ created with 5 file names from d:\wpswrkx.
    NOTE: File attributes were requested and have been added to __want__. Variable
    names are RECFM, LRECL, File_Size__bytes_, Last_Modified
    NOTE: PROC PRINT data=__want__

    NOTE: PROC PRINT completed: 5 observations printed, 7 variables
    NOTE: DATA _null_


    NOTE: Read 1 rows from __want__.
    NOTE: DATA elapsed:
      wall  0.00 seconds
      cpu   0.00 seconds
    NOTE: PROC CONTENTS data=d:\wpswrkx\iris.parquet

    NOTE: PROC CONTENTS completed: 150 observations, 5 variables


    /******************************************************************************************************************/
    /*  4 MACROS   (utl_sdrContents  and contentsx )                                                                  */
    /******************************************************************************************************************/

    filename ft15f001 "c:/otojnr/utl_dirContents.sas";
    parmcards4;
    %macro utl_dirContents(
     dir= /* directory name to process */
    , ext= /* optional extension to filter on */
    , dsout=work.dir_contents /* dataset name to hold the file names */
    , attribs=N /* get file attributes? (Y/N ) */
    );
    %global _dir_fileN;
    %local _syspathdlim _exitmsg _attrib_vars;
    %* verify the required parameter has been provided. ;
    %if %length(&dir) = 0 %then %do;
     %let _exitmsg = %str(E)RROR: No directory name specified - macro will exit.;
     %goto finish;
    %end;
    %* verify existence of the requested directory name. ;
    %if %sysfunc(fileexist(&dir)) = 0 %then %do;
     %let _exitmsg = %str(E)RROR: Specified input location, &dir., does not exist - macro
    will exit.;
     %goto finish;
    %end;
    %* set the separator character needed for the full file path: ;
    %* (backslash for Windows, forward slash for UNIX systems) ;
    %if &sysscp = WIN %then %do;
     %let _syspathdlim = \;
    %end;
    %else %do;
     %let _syspathdlim = /;
    %end;
    /*--- begin data step to capture names of all file names found in the specified
    directory. ---*/
    data &dsout(keep=file_seq basefile pathname);
     length basefile $ 40 pathname $ 1000 _msg $ 1000;
     /* Allocate directory */
     rc=FILENAME('xdir', "&dir");

     if rc ne 0 then do;
     _msg = "E" || 'RROR: Unable to assign fileref to specified directory. ' ||
    sysmsg();
     go to finish_datastep;
     end;
     /* Open directory */
     dirid=DOPEN('xdir');
     if dirid eq 0 then do;
     _msg = "E" || 'RROR: Unable to open specified directory. ' || sysmsg();
     go to finish_datastep;
     end;
     /* Get number of information items */
     nfiles=DNUM(dirid);
     do j = 1 to nfiles;
     basefile = dread(dirid, j);
     pathname=strip("&dir") || "&_syspathdlim." || strip(basefile);

     %if %length(&ext) %then %do;
    /* scan the final "word" of the full file name, delimited by dot character. */

     ext = scan(basefile,-1,'.');
     if ext="&ext." then do;
     file_seq + 1;
     output;
     end;
     %end;
     %else %do;
     file_seq + 1;
     output;
     %end;
     end;
     /* Close the directory */
     rc=DCLOSE(dirid);
     /* Deallocate the directory */
     rc=FILENAME('xdir');

     call symputx('_dir_fileN', file_seq);
     finish_datastep:
     if _msg ne ' ' then do;
     call symput('_exitmsg', _msg);
     end;
    run;
    %if %upcase(&attribs)=Y and &_dir_fileN > 0 %then %do;
     data _file_attr(keep=file_seq basefile infoname infoval);
     length infoname infoval $ 500;
     set &dsout.;
    /* open each file to get the additional attributes available. */
     rc=filename("afile", pathname);
     fid=fopen("afile");
    /* return the number of system-dependent information items available for the
    external file. */
     infonum=foptnum(fid);
    /* loop to get the name and value of each information item. */
     do i=1 to infonum;
     infoname=foptname(fid,i);
     infoval=finfo(fid,infoname);
     if upcase(infoname) ne 'FILENAME' then output;
     end;
     close=fclose(fid);
     run;
     /* transpose each information item into its own variable */
     proc transpose data=_file_attr out=trans_attr(drop=_:) ;
     by file_seq basefile ;
     var infoval;
     id infoname;
     run;
     proc sql noprint;
     select distinct name into : _attrib_vars separated by ', '
     from dictionary.columns
     where memname='TRANS_ATTR' and upcase(name) not in('BASEFILE', 'FILE_SEQ')
     order by varnum;
     quit;
     /* merge back the additional attributes to the related file name. */
     data &dsout.;
     merge &dsout. trans_attr;
     by file_seq basefile;
     run;
     proc datasets nolist memtype=data lib=work;
     delete _file_attr trans_attr;
     run;
     quit;
    %end;
    %if %length(&_exitmsg) = 0 %then
    %let _exitmsg = NOTE: &dsout created with &_dir_fileN. file names ;
    %if %length(&ext) %then
    %let _exitmsg = &_exitmsg where extension is equal to &ext.;
    %let _exitmsg = &_exitmsg from &dir..;
    %finish:
    %put &_exitmsg;
    %if %length(&_attrib_vars) ne 0 %then %do;
     %put;
     %put NOTE: File attributes were requested and have been added to &dsout.. Variable
    names are &_attrib_vars.;
    %end;
    %mend utl_dirContents;
    ;;;;
    run;



    filename ft15f001 "c:/otojnr/contentsx.sas";
    parmcards4;
    %macro contentsx(libname=workx,member=all)
       /des="proc contents for libraries of mixed data formats parquet avro sas7bdat";

       %local pathname;

       %let pathname=%qsysfunc(pathname(&libname));
       %utl_dirContents(dir=&pathname,dsout=__want__);

       %* roc print data=__want__;
       %* run;

       %if %upcase(&member=ALL) %then %do;

       data _null_;
         set __want__(keep=pathname basefile);
         call execute(cats("proc contents data='",pathname,"';run;"));
       run;
       %end;
       %else %do;
       data _null_;
         set __want__(keep=pathname basefile where=(upcase("&member") =: upcase(basefile)));
         call execute(cats("proc contents data='",pathname,"';run;"));

       %end;

    run;

    %mend contentsx;
    ;;;;
    run;


    /*--- END ---*/
    
