# utl_moving_average_of_centered_timeseries_or_calculate_a_modified_version_of_moving_averages
Moving average of centered timeseries or calculate a modified version of moving averages  You may not want to roll your own. There are many R packages for timeseries, ie zoo, TTR and forecast.
    Moving average of centered timeseries or calculate a modified version of moving averages                                            
                                                                                                                                        
    You may not want to roll your own. There are many R packages for                                                                    
    timeseries, ie zoo, TTR and forecast.                                                                                               
                                                                                                                                        
    see                                                                                                                                 
    https://goo.gl/ntTQ21                                                                                                               
    https://communities.sas.com/t5/General-SAS-Programming/Calculate-a-modified-version-of-moving-averages/m-p/417672                   
                                                                                                                                        
    INPUT                                                                                                                               
    =====                                                                                                                               
                                                                                                                                        
      SD1.HAVE total obs=8            RULES                                                                                             
                                      =====                                                                                             
        Obs    WEEK    SALES                                                                                                            
                                                                                                                                        
         1       1       792           .   need two prior readings                                                                      
         2       2       271                                                                                                            
         3       3       490       817.4   (792+271+490+1095+1439)/5                                                                    
         4       4      1095       778.4   (271+490+1095+1439+597)/5                                                                    
         5       5      1439                                                                                                            
         6       6       597                                                                                                            
         7       7       588                                                                                                            
         8       8      7078                                                                                                            
                                                                                                                                        
    WORKING CODE                                                                                                                        
    ============                                                                                                                        
                                                                                                                                        
         ma(have$SALES, order=5);                                                                                                       
                                                                                                                                        
    OUTPUT                                                                                                                              
    ======                                                                                                                              
                                                                                                                                        
       WORK.WANTWPS total obs=8                                                                                                         
                                                                                                                                        
         WEEK    SALES       X                                                                                                          
                                                                                                                                        
           1       792        .                                                                                                         
           2       271        .                                                                                                         
           3       490     817.4                                                                                                        
           4      1095     778.4                                                                                                        
           5      1439     841.8                                                                                                        
           6       597    2159.4                                                                                                        
           7       588        .                                                                                                         
           8      7078        .                                                                                                         
                                                                                                                                        
    *                _              _       _                                                                                           
     _ __ ___   __ _| | _____    __| | __ _| |_ __ _                                                                                    
    | '_ ` _ \ / _` | |/ / _ \  / _` |/ _` | __/ _` |                                                                                   
    | | | | | | (_| |   <  __/ | (_| | (_| | || (_| |                                                                                   
    |_| |_| |_|\__,_|_|\_\___|  \__,_|\__,_|\__\__,_|                                                                                   
    ;                                                                                                                                   
                                                                                                                                        
    options validvarname=upcase;                                                                                                        
    libname sd1 "d:/sd1";                                                                                                               
    data sd1.have;                                                                                                                      
    input week sales;                                                                                                                   
    cards4;                                                                                                                             
    1 792                                                                                                                               
    2 271                                                                                                                               
    3 490                                                                                                                               
    4 1095                                                                                                                              
    5 1439                                                                                                                              
    6 597                                                                                                                               
    7 588                                                                                                                               
    8 7078                                                                                                                              
    ;;;;                                                                                                                                
    run;quit;                                                                                                                           
                                                                                                                                        
    *          _       _   _                                                                                                            
     ___  ___ | |_   _| |_(_) ___  _ __                                                                                                 
    / __|/ _ \| | | | | __| |/ _ \| '_ \                                                                                                
    \__ \ (_) | | |_| | |_| | (_) | | | |                                                                                               
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|                                                                                               
    ;                                                                                                                                   
                                                                                                                                        
                                                                                                                                        
    %utl_submit_wps64('                                                                                                                 
    libname sd1 "d:/sd1";                                                                                                               
    options set=R_HOME "C:/Program Files/R/R-3.3.1";                                                                                    
    libname wrk "%sysfunc(pathname(work))";                                                                                             
    proc r;                                                                                                                             
    submit;                                                                                                                             
    source("C:/Program Files/R/R-3.3.2/etc/Rprofile.site", echo=T);                                                                     
    library(haven);                                                                                                                     
    library(forecast);                                                                                                                  
    have<-read_sas("d:/sd1/have.sas7bdat");                                                                                             
    head(have);                                                                                                                         
    want<-c(have,as.data.frame(ma(have$SALES, order=5)));                                                                               
    endsubmit;                                                                                                                          
    import r=want  data=wrk.wantwps;                                                                                                    
    run;quit;                                                                                                                           
    ');                                                                                                                                 
                                                                                                                                        
                                                                                                                                        
    * another test case;                                                                                                                
                                                                                                                                        
    %utl_submit_r64('                                                                                                                   
    library("forecast");                                                                                                                
    library(haven);                                                                                                                     
    ts<-c(-1,0,1,-1,0,1,-1,0,1,-1,0,1,-1,0,1);                                                                                          
    ma(ts, order=3);                                                                                                                    
    ');                                                                                                                                 
                                                                                                                                        
    Time Series:                                                                                                                        
    Start = 1                                                                                                                           
    End = 15                                                                                                                            
    Frequency = 1                                                                                                                       
     [1] NA  0  0  0  0  0  0  0  0  0  0  0  0  0 NA                                                                                   
                                                                                                                                        
