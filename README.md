# utl-generate-all-possible-paiwise-interactions-products-regression
Generate all possible paiwise interactions products regression
    Generate all possible paiwise interactions products regression

    github
    https://tinyurl.com/yagkl4qw
    https://github.com/rogerjdeangelis/utl-generate-all-possible-paiwise-interactions-products-regression

    macros
    https://tinyurl.com/y9nfugth
    https://github.com/rogerjdeangelis/utl-macros-used-in-many-of-rogerjdeangelis-repositories



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




