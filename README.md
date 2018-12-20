# utl-generate-all-possible-paiwise-interactions-products-regression
Generate all possible paiwise interactions products regression
    Generate all possible paiwise interactions products regression

    github
    https://tinyurl.com/yagkl4qw
    https://github.com/rogerjdeangelis/utl-generate-all-possible-paiwise-interactions-products-regression

    macros
    https://tinyurl.com/y9nfugth
    https://github.com/rogerjdeangelis/utl-macros-used-in-many-of-rogerjdeangelis-repositories

    Recent solution by Rick and data_null_ (see Ricks comments and links on end)

    Rick Wicklin
    rick.wicklin@sas.com
    PhD, Applied mathematics Cornell University
    Blog on Statistical Programming in SAS

    data _null_,
    datanull@gmail.com

    SOLUTION
    --------
    ods trace on;
    ods output designpoints=xmat;
    ods output parameters=coef;
    proc glmmod data=sashelp.class;
       model age = age|height|weight @2;
       run;
    ods trace off;


    WORK.XMAT total obs=19

                                                AGE_                AGE_      HEIGHT_
    Obs    OBSNUMBER    AGE   AGE2    HEIGHT    HEIGHT    WEIGHT    WEIGHT     WEIGHT

      1         1        14    14      69.0      966.0     112.5    1575.0     7762.50
      2         2        13    13      56.5      734.5      84.0    1092.0     4746.00
      3         3        13    13      65.3      848.9      98.0    1274.0     6399.40
    ....
     17        17        15    15      67.0     1005.0     133.0    1995.0     8911.00
     18        18        11    11      57.5      632.5      85.0     935.0     4887.50
     19        19        15    15      66.5      997.5     112.0    1680.0     7448.00



    WORK.COEF total obs=7

      COLUMNNUMBER    EFFECT

            1         Intercept
            2         AGE
            3         HEIGHT
            4         AGE*HEIGHT
            5         WEIGHT
            6         AGE*WEIGHT
            7         HEIGHT*WEIGHT

    INPUT
    =====

     WORK.HAVE total obs=2

                     |            RULES
                     | Add this interactions (products)
                     |
     A  B  C  D  E   |  AB AC AD AE BC BD BE CD CE DE
                     |
     0  1  2  3  4   |   0  0  0  0  2  3  4  6  8 12
     0  1  2  3  4   |   0  0  0  0  2  3  4  6  8 12


    EXAMPLE OUTPUT
    --------------

     WORK.WANT total obs=2

     A  B  C  D  E AB AC AD AE BC BD BE CD CE DE

     0  1  2  3  4  0  0  0  0  2  3  4  6  8 12
     0  1  2  3  4  0  0  0  0  2  3  4  6  8 12


    PROCESS
    =======

    * just for development;
    proc datasets lib=work;
      delete want;
    run;quit;

    %symdel muln nuvn mul1 nuv1 / nowarn;

    data want;

      * create the meta data;
      if _n_=0 then do; %let rc=%sysfunc(dosubl('
         data _null_;
            set have(obs=1);
            array xs _numeric_;
            idx=0;
            do i=1 to dim(xs);
              do j=i to dim(xs);
                if vname(xs[i]) ne vname(xs[j]) then do;
                   idx=idx+1;
                   call symputx(cats("nuv",idx),cats(vname(xs[i]),vname(xs[j])));
                   call symputx(cats("mul",idx),cats(vname(xs[i]),'*',vname(xs[j])));
                end;
              end;
            end;
            call symputx("nuvn",dim(xs)*2);
            call symputx("muln",dim(xs)*2);
            stop;
         run;quit;
       '));
      end ;

      set have;

      %do_over(nuv mul,phrase=%str(?nuv = ?mul;))

    run;quit;

    * generated runtime code (output of my debug macro);

    options mfile mprint source2;
    data want;
    * create the meta data;
    if _n_=0 then do;
    end ;
    set have;
    AB = A*B;
    AC = A*C;
    AD = A*D;
    AE = A*E;
    BC = B*C;
    BD = B*D;
    BE = B*E;
    CD = C*D;
    CE = C*E;
    DE = D*E;
    run;
    quit;
    run;
    quit;

    *                _               _       _
     _ __ ___   __ _| | _____     __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \   / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/  | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|   \__,_|\__,_|\__\__,_|

    ;

    data have;
     array xs[5] a b c d e (0,1,2,3,4);
     output;
     output;
    run;quit;


    *____  _      _      ____        _                        _ _
    |  _ \(_) ___| | __ |  _ \  __ _| |_ __ _     _ __  _   _| | |
    | |_) | |/ __| |/ / | | | |/ _` | __/ _` |   | '_ \| | | | | |
    |  _ <| | (__|   <  | |_| | (_| | || (_| |   | | | | |_| | | |
    |_| \_\_|\___|_|\_\ |____/ \__,_|\__\__,_|___|_| |_|\__,_|_|_|____
                                            |_____|             |_____|
    ;


    Recent solution by Rick and data_null_ (see Ricks comments and links on end)

    Rick Wicklin
    rick.wicklin@sas.com
    PhD, Applied mathematics Cornell University
    Blog on Statistical Programming in SAS

    data _null_,
    datanull@gmail.com

    SOLUTION
    --------
    ods trace on;
    ods output designpoints=xmat;
    ods output parameters=coef;
    proc glmmod data=sashelp.class;
       model age = age|height|weight @2;
       run;
    ods trace off;


    WORK.XMAT total obs=19

                                                AGE_                AGE_      HEIGHT_
    Obs    OBSNUMBER    AGE   AGE2    HEIGHT    HEIGHT    WEIGHT    WEIGHT     WEIGHT

      1         1        14    14      69.0      966.0     112.5    1575.0     7762.50
      2         2        13    13      56.5      734.5      84.0    1092.0     4746.00
      3         3        13    13      65.3      848.9      98.0    1274.0     6399.40
    ....
     17        17        15    15      67.0     1005.0     133.0    1995.0     8911.00
     18        18        11    11      57.5      632.5      85.0     935.0     4887.50
     19        19        15    15      66.5      997.5     112.0    1680.0     7448.00



    WORK.COEF total obs=7

      COLUMNNUMBER    EFFECT

            1         Intercept
            2         AGE
            3         HEIGHT
            4         AGE*HEIGHT
            5         WEIGHT
            6         AGE*WEIGHT
            7         HEIGHT*WEIGHT

    *____  _      _
    |  _ \(_) ___| | __
    | |_) | |/ __| |/ /
    |  _ <| | (__|   <
    |_| \_\_|\___|_|\_\

    ;

    It sounds like you are looking for a way to generate a design matrix
    that includes main an interaction effects. Since you are apparently already
    familiar with GLMSELECT, you can use that to generate the design matrix.
    The nice thing about GLMSELECT is that you can obtain a macro variable that
    gives you the names of the interaction terms. See the discussion and code at
    https://blogs.sas.com/content/iml/2018/07/30/names-columns-design-matrix.html

    There are other options, which are described in the article
    "Four ways to create a design matrix in SAS":
    https://blogs.sas.com/content/iml/2016/02/24/create-a-design-matrix-in-sas.html

    Regarding running hundreds of univariate regressions, you can do this by
    using your favorite regression procedure by using a BY-group approach, as explained at
    https://blogs.sas.com/content/iml/2017/02/13/run-1000-regressions.html

    If the data will all fit in memory, you can also perform the analysis by using
    a single statement (SWEEP) in SAS/IML, as described in
    https://blogs.sas.com/content/iml/2018/04/25/sweep-thousands-of-regressions.html

    Best wishes,
    Rick Wicklin







