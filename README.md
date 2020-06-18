# utl-fast-join-small_1g-table_with-a-moderate_50gb-tables-hash-sql
Possibly reduced the time from 30 minutes to 25 seconds
    Possibly reduced the time from 30 minutes to 25 seconds.                                                                
                                                                                                                            
    github                                                                                                                  
    https://tinyurl.com/y9h9zbgg                                                                                            
    https://github.com/rogerjdeangelis/utl-fast-join-small_1g-table_with-a-moderate_50gb-tables-hash-sql                    
                                                                                                                            
    sas forum                                                                                                               
    https://tinyurl.com/yawhw7s2                                                                                            
    https://communities.sas.com/t5/SAS-Programming/Joining-two-very-large-tables/m-p/661685                                 
                                                                                                                            
    There are cases where the hash cam outperform SQL.                                                                      
    It depends on locality of reference and if you just need to modify the bigger table.                                    
                                                                                                                            
    Fast join small and moderate size tables hash                                                                           
                                                                                                                            
          Two Solutions              Seconds                                                                                
                                                                                                                            
              a. sql                   25                                                                                   
              b. single hash          101                                                                                   
              c. muti-task hash        41  * could optimize                                                                 
                                                                                                                            
                                                                                                                            
    "I have two tables, each with about 4-5 million rows of data.                                                           
    Table A has about 50 columns and table B 2500.                                                                          
    The key is a combination of 4 columns."                                                                                 
                                                                                                                            
                                                                                                                            
    INPUT                                                                                                                   
    =====                                                                                                                   
                            Number                                                                                          
      Table      Rows       Size   Columns        Columns           Primary keu                                             
                                                                                                                            
      small    4,000,000     1gb      54      cx1-cx25 nx1-nx25     Year states zip urban_rural                             
      medium   4,000,000    50gb    2504      c1-c1250 n1-n1250     Year states zip urban_rural                             
                                                                                                                            
    OUTPUT (10% MATCH)                                                                                                      
    ==================                                                                                                      
                                                                                                                            
      want       400,000    225mb    114      cx1-cx5 nx1-nx5       Year staes zip urban_rural                              
                                                                                                                            
                                                                                                                            
    *_                   _                                                                                                  
    (_)_ __  _ __  _   _| |_                                                                                                
    | | '_ \| '_ \| | | | __|                                                                                               
    | | | | | |_) | |_| | |_                                                                                                
    |_|_| |_| .__/ \__,_|\__|                                                                                               
            |_|                                                                                                             
    ;                                                                                                                       
                                                                                                                            
    * from my autoexec file;                                                                                                
                                                                                                                            
    %let states="AL","AK","AZ","AR","CA","CO","CT","DE","FL","GA","HI","ID","IL","IN"                                       
    ,"IA","KS","KY","LA","ME","MD","MA","MI","MN","MS","MO","MT"                                                            
    ,"NE","NV","NH","NJ","NM","NY","NC","ND","OH","OK","OR","PA","RI"                                                       
    ,"SC","SD","TN","TX","UT","VT","VA","WA","WV","WI","WY";                                                                
                                                                                                                            
    libname spde spde "f:/wrk";                                                                                             
                                                                                                                            
    data spde.small(compress=no);                                                                                           
                                                                                                                            
     array chr[25] $5 cx1-cx25 (25*"ROGER");                                                                                
     array num[25] $5 nx1-nx25 (25*"5");                                                                                    
                                                                                                                            
     do year="2010" ,"2011" ,"2012" ,"2013" ,"2014" ,"2015" ,"2016" ,"2017" ,"2018" ,"2019";                                
       do states=&states;                                                                                                   
         do zipcode=0 to 4000 by 1;                                                                                         
           zip = put(zipcode,z5.);                                                                                          
           do urban_rural="U","R";                                                                                          
             output;                                                                                                        
           end;                                                                                                             
         end;                                                                                                               
       end;                                                                                                                 
     end;                                                                                                                   
                                                                                                                            
     drop zipcode;                                                                                                          
                                                                                                                            
     stop;                                                                                                                  
                                                                                                                            
    run;quit;                                                                                                               
                                                                                                                            
    /*                                                                                                                      
    SPDE.SMALL                                                                                                              
                                                                                                                            
    Data Set Name        SPDE.SMALL                    Observations          4001000                                        
    Member Type          DATA                          Variables             54                                             
    Engine               SPDE                          Indexes               0                                              
    Created              06/18/2020 09:06:43           Observation Length    262                                            
                                                                                                                            
      Engine/Host Dependent Information                                                                                     
                                                                                                                            
    Blocking Factor (obs/block)  4002                                                                                       
    Data Partsize                134211072                                                                                  
    */                                                                                                                      
                                                                                                                            
    data spde.medium(compress=no);                                                                                          
                                                                                                                            
     array chr[1250] $5 c1-c1250 (1250*"SMITH");                                                                            
     array num[1250] $5 n1-n1250 (1250*"9");                                                                                
                                                                                                                            
     do year="2010" ,"2011" ,"2012" ,"2013" ,"2014" ,"2015" ,"2016" ,"2017" ,"2018" ,"2019";                                
       do states=&states;                                                                                                   
         do zipcode=0 to 40000 by 1;                                                                                        
           zip = put(zipcode,z5.);                                                                                          
           do urban_rural="U","R";                                                                                          
             if uniform(4321) le .1 then output;                                                                            
           end;                                                                                                             
         end;                                                                                                               
       end;                                                                                                                 
     end;                                                                                                                   
                                                                                                                            
     drop zipcode;                                                                                                          
                                                                                                                            
     stop;                                                                                                                  
    run;quit;                                                                                                               
                                                                                                                            
    /*                                                                                                                      
    Data Set Name        SPDE.MEDIUM                   Observations          4000008                                        
    Member Type          DATA                          Variables             2504                                           
    Engine               SPDE                          Indexes               1                                              
    Created              06/18/2020 08:24:06           Observation Length    12512                                          
                                                                                                                            
                                                                                                                            
            Engine/Host Dependent Information                                                                               
                                                                                                                            
    Blocking Factor (obs/block)        83                                                                                   
    Data Partsize                      134128640                                                                            
    -   Alphabetic List of Index Info  -                                                                                    
    Index                              KEY                                                                                  
    KeyValue (Min)                     2010,AK,00001,U                                                                      
    KeyValue (Max)                     2019,WY,40000,U                                                                      
    Number of discrete values          4000008                                                                              
    */                                                                                                                      
                                                                                                                            
    *            _               _                                                                                          
      ___  _   _| |_ _ __  _   _| |_                                                                                        
     / _ \| | | | __| '_ \| | | | __|                                                                                       
    | (_) | |_| | |_| |_) | |_| | |_                                                                                        
     \___/ \__,_|\__| .__/ \__,_|\__|                                                                                       
                    |_|                                                                                                     
    ;                                                                                                                       
                                                                                                                            
    Data Set Name        WORK.WANT_ALL                 Observations          400078                                         
    Member Type          DATA                          Variables             114                                            
    Engine               V9                            Indexes               0                                              
    Created              06/18/2020 09:37:01           Observation Length    562                                            
                                                                                                                            
                     Engine/Host Dependent Information                                                                      
                                                                                                                            
    Data Set Page Size          65536                                                                                       
    Number of Data Set Pages    3450                                                                                        
    First Data Page             1                                                                                           
    Max Obs per Page            116                                                                                         
    Obs in First Data Page      96                                                                                          
    Number of Data Set Repairs  0                                                                                           
    ExtendObsCounter            YES                                                                                         
    Filename                    f:\wrk\_TDxxxxxxxxxxx\want_all.sas7bdat                                                     
    Release Created             9.0401M6                                                                                    
    Host Created                X64_10PRO                                                                                   
    Owner Name                  T7610\xxxxx                                                                                 
    File Size                   216MB                                                                                       
    File Size (bytes)           226164736                                                                                   
                                                                                                                            
    *          _       _   _                                                                                                
     ___  ___ | |_   _| |_(_) ___  _ __  ___                                                                                
    / __|/ _ \| | | | | __| |/ _ \| '_ \/ __|                                                                               
    \__ \ (_) | | |_| | |_| | (_) | | | \__ \                                                                               
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|___/                                                                               
                       _                                                                                                    
      __ _   ___  __ _| |                                                                                                   
     / _` | / __|/ _` | |                                                                                                   
    | (_| |_\__ \ (_| | |                                                                                                   
     \__,_(_)___/\__, |_|                                                                                                   
                    |_|                                                                                                     
    ;                                                                                                                       
                                                                                                                            
    proc sql;                                                                                                               
      create unique index key on spde.medium (Year, states, zip, urban_rural)                                               
    ;quit;                                                                                                                  
                                                                                                                            
    /*                                                                                                                      
    572   proc sql;                                                                                                         
    573     create unique index key on spde.medium (Year, states, zip, urban_rural)                                         
    574   ;                                                                                                                 
    NOTE: Composite index key has been defined.                                                                             
    574 !  quit;                                                                                                            
    NOTE: PROCEDURE SQL used (Total process time):                                                                          
          real time           4.71 seconds                                                                                  
          cpu time            1:45.84                                                                                       
    */                                                                                                                      
                                                                                                                            
    %utlnopts;                                                                                                              
    %array(med,values=1-50);                                                                                                
    %array(smal,values=1-5);                                                                                                
                                                                                                                            
    %put &=med26;                                                                                                           
                                                                                                                            
    %utlopts;                                                                                                               
    proc sql;                                                                                                               
      create                                                                                                                
         table want_all as                                                                                                  
      select                                                                                                                
          l.Year                                                                                                            
         ,l.states                                                                                                          
         ,l.zip                                                                                                             
         ,l.urban_rural                                                                                                     
         ,%do_over(smal,phrase=l.cx?,between=comma)                                                                         
         ,%do_over(smal,phrase=l.nx?,between=comma)                                                                         
         ,%do_over(med,phrase=r.c?,between=comma)                                                                           
         ,%do_over(med,phrase=r.n?,between=comma)                                                                           
      from                                                                                                                  
         spde.small as l, spde.medium as r                                                                                  
      where                                                                                                                 
         l.Year        = r.Year          and                                                                                
         l.states      = r.states        and                                                                                
         l.zip         = r.zip           and                                                                                
         l.urban_rural = r.urban_rural                                                                                      
    ;quit;                                                                                                                  
                                                                                                                            
                                                                                                                            
    NOTE: Table WORK.WANT_ALL created, with 400078 rows and 114 columns.                                                    
                                                                                                                            
    850 !  quit;                                                                                                            
    NOTE: PROCEDURE SQL used (Total process time):                                                                          
          real time           19.87 seconds                                                                                 
          user cpu time       3.14 seconds                                                                                  
          system cpu time     16.15 seconds                                                                                 
          memory              407901.95k                                                                                    
          OS Memory           432948.00k                                                                                    
          Timestamp           06/18/2020 09:37:17 AM                                                                        
          Step Count                        75  Switch Count  0                                                             
                                                                                                                            
    *_                           _               _                                                                          
    | |__      ___  _ __   ___  | |__   __ _ ___| |__                                                                       
    | '_ \    / _ \| '_ \ / _ \ | '_ \ / _` / __| '_ \                                                                      
    | |_) |  | (_) | | | |  __/ | | | | (_| \__ \ | | |                                                                     
    |_.__(_)  \___/|_| |_|\___| |_| |_|\__,_|___/_| |_|                                                                     
                                                                                                                            
    ;                                                                                                                       
                                                                                                                            
    Options obs=max;                                                                                                        
    data want (drop = key) ;                                                                                                
      dcl hash h(hashexp:20, multidata:"y") ;                                                                               
      h.definekey ("key") ;                                                                                                 
      h.definedata ("rid") ;                                                                                                
      h.definedone() ;                                                                                                      
      length key $ 15 ;                                                                                                     
      do rid = 1 by 1 until (z1) ;                                                                                          
                                                                                                                            
        set spde.medium ( keep=Year -- urban_rural) end = z1 ;                                                              
                                                                                                                            
        key = catx (":", of Year -- urban_rural) ;                                                                          
        h.add() ;                                                                                                           
      end ;                                                                                                                 
      do until (z2) ;                                                                                                       
                                                                                                                            
        set spde.small(keep=Year -- urban_rural cx1-cx5 nx1-nx5)  end = z2 ;                                                
                                                                                                                            
        key = catx (":", of Year -- urban_rural) ;                                                                          
        do _iorc_ = h.find() by 0 while (_iorc_ = 0) ;                                                                      
                                                                                                                            
          set spde.medium(keep=Year -- urban_rural c1-c50  n1-n50) point = rid ;                                            
                                                                                                                            
          output ;                                                                                                          
          _iorc_ = h.find_next() ;                                                                                          
        end ;                                                                                                               
      end ;                                                                                                                 
      stop ;                                                                                                                
    run;quit;                                                                                                               
                                                                                                                            
    /*                                                                                                                      
    NOTE: There were 4000008 observations read from the data set SPDE.MEDIUM.                                               
    NOTE: There were 4001000 observations read from the data set SPDE.SMALL.                                                
    NOTE: The data set WORK.WANT has 400078 observations and 114 variables.                                                 
    NOTE: DATA statement used (Total process time):                                                                         
          real time           1:41.08                                                                                       
          cpu time            1:41.06                                                                                       
    */                                                                                                                      
                                                                                                                            
    *_                                      _               _                                                               
    | |__     _ __ ___   __ _ _ __  _   _  | |__   __ _ ___| |__                                                            
    | '_ \   | '_ ` _ \ / _` | '_ \| | | | | '_ \ / _` / __| '_ \                                                           
    | |_) |  | | | | | | (_| | | | | |_| | | | | | (_| \__ \ | | |                                                          
    |_.__(_) |_| |_| |_|\__,_|_| |_|\__, | |_| |_|\__,_|___/_| |_|                                                          
                                    |___/                                                                                   
    ;                                                                                                                       
                                                                                                                            
                                                                                                                            
    filename ft15f001 "c:/oto/utl_hshTsk.sas";                                                                              
    parmcards4;                                                                                                             
    %macro utl_hshTsk(year);                                                                                                
                                                                                                                            
    libname spde spde "f:/wrk";                                                                                             
    libname sd1 "f:/wrk";                                                                                                   
                                                                                                                            
    data sd1.want_&year (drop = key) ;                                                                                      
      dcl hash h(hashexp:20, multidata:"y") ;                                                                               
      h.definekey ("key") ;                                                                                                 
      h.definedata ("rid") ;                                                                                                
      h.definedone() ;                                                                                                      
      length key $ 15 ;                                                                                                     
      do rid = 1 by 1 until (z1) ;                                                                                          
                                                                                                                            
        set spde.medium (where=(year="&year") keep=Year -- urban_rural) end = z1 ;                                          
                                                                                                                            
        key = catx (":", of Year -- urban_rural) ;                                                                          
                                                                                                                            
        h.add() ;                                                                                                           
      end ;                                                                                                                 
      do until (z2) ;                                                                                                       
                                                                                                                            
        set spde.small(keep=Year -- urban_rural cx1-cx5 nx1-nx5)  end = z2 ;                                                
                                                                                                                            
        key = catx (":", of Year -- urban_rural) ;                                                                          
        do _iorc_ = h.find() by 0 while (_iorc_ = 0) ;                                                                      
                                                                                                                            
          set spde.medium(keep=Year -- urban_rural c1-c50  n1-n50) point = rid ;                                            
                                                                                                                            
          output ;                                                                                                          
          _iorc_ = h.find_next() ;                                                                                          
        end ;                                                                                                               
      end ;                                                                                                                 
      stop ;                                                                                                                
    run;quit;                                                                                                               
    %mend utl_hshTsk;                                                                                                       
    ;;;;                                                                                                                    
    run;quit;                                                                                                               
                                                                                                                            
    * for systask coding;                                                                                                   
    %let _s=%sysfunc(compbl(&_r\PROGRA~1\SASHome\SASFoundation\9.4\sas.exe -sysin nul -log nul -work &_r\wrk                
       -rsasuser -nosplash -sasautos &_r\oto -RLANG -config &_r\cfg\cfgsas94m6.cfg));                                       
                                                                                                                            
    options obs=100000;                                                                                                     
    %utl_hshTsk(2010);                                                                                                      
                                                                                                                            
                                                                                                                            
    %include "c:/oto/utl_hshTsk.sas" /source;                                                                               
    %utl_hshTsk(2010);                                                                                                      
                                                                                                                            
    systask kill sys0 sys1 sys2 sys3 sys4  sys5 sys6 sys7 sys8 sys9 ;                                                       
    systask command "&_s -termstmt %nrstr(%utl_hshTsk(2010);) -log d:\log\a0.log" taskname=sys0;                            
    systask command "&_s -termstmt %nrstr(%utl_hshTsk(2011);) -log d:\log\a1.log" taskname=sys1;                            
    systask command "&_s -termstmt %nrstr(%utl_hshTsk(2012);) -log d:\log\a2.log" taskname=sys2;                            
    systask command "&_s -termstmt %nrstr(%utl_hshTsk(2013);) -log d:\log\a3.log" taskname=sys3;                            
    systask command "&_s -termstmt %nrstr(%utl_hshTsk(2014);) -log d:\log\a4.log" taskname=sys4;                            
    systask command "&_s -termstmt %nrstr(%utl_hshTsk(2015);) -log d:\log\a5.log" taskname=sys5;                            
    systask command "&_s -termstmt %nrstr(%utl_hshTsk(2016);) -log d:\log\a6.log" taskname=sys6;                            
    systask command "&_s -termstmt %nrstr(%utl_hshTsk(2017);) -log d:\log\a7.log" taskname=sys7;                            
    systask command "&_s -termstmt %nrstr(%utl_hshTsk(2018);) -log d:\log\a8.log" taskname=sys8;                            
    systask command "&_s -termstmt %nrstr(%utl_hshTsk(2019);) -log d:\log\a9.log" taskname=sys9;                            
    waitfor sys0 sys1 sys2 sys3 sys4  sys5 sys6 sys7 sys8 sys9                                                              
                                                                                                                            
    * beter to keep the pieces and materize if needed;                                                                      
    data want_hash/view=want_hash;                                                                                          
       set                                                                                                                  
         spde.want_2010                                                                                                     
         spde.want_2011                                                                                                     
         spde.want_2012                                                                                                     
         spde.want_2013                                                                                                     
         spde.want_2014                                                                                                     
         spde.want_2015                                                                                                     
         spde.want_2016                                                                                                     
         spde.want_2017                                                                                                     
         spde.want_2018                                                                                                     
         spde.want_2019                                                                                                     
    ;run;quit;                                                                                                              
                                                                                                                            
    /* sample log                                                                                                           
    NOTE: Libref SPDE was successfully assigned as follows:                                                                 
          Engine:        SPDE                                                                                               
          Physical Name: f:\wrk\                                                                                            
                                                                                                                            
    NOTE: There were 400739 observations read from the data set SPDE.MEDIUM.                                                
          WHERE year='2016';                                                                                                
    NOTE: There were 4001000 observations read from the data set SPDE.SMALL.                                                
    NOTE: The data set WORK.WANT_2016 has 40157 observations and 114 variables.                                             
    NOTE: DATA statement used (Total process time):                                                                         
          real time           41.73 seconds                                                                                 
          cpu time            2:01.39                                                                                       
                                                                                                                            
    */                                                                                                                      
                                                                                                                            
                                                                                                                            
