# utl-parse-out-and-standardize-business-types-ie-Ltd-becomes-Limited
Parse-out-and-standardize-business-types-ie-Ltd-becomes-Limited  

    Parse-out-and-standardize-business-types-ie-Ltd-becomes-Limited                                                      
                                                                                                                         
    github                                                                                                               
    https://tinyurl.com/whwaytt                                                                                          
    https://github.com/rogerjdeangelis/utl-parse-out-and-standardize-business-types-ie-Ltd-becomes-Limited               
                                                                                                                         
    Related to SAS Forum                                                                                                 
    https://tinyurl.com/yxx5l2n2                                                                                         
    https://communities.sas.com/t5/SAS-Data-Management/Data-cleansing-company-names/m-p/606109/highlight/true#M18443     
                                                                                                                         
    You should google Pytho business company names                                                                       
                                                                                                                         
         Two solutions                                                                                                   
             a. Simple single case                                                                                       
             b. Table of Business Names                                                                                  
                                                                                                                         
    * Personally I think the downfall of R and Python will be the                                                        
      plethora of data structures. Less is More. Most packages seem to input                                             
      and output Python and R data structures that are very different from SAS/WPS and                                   
      relational databases. It took me a long time to convert the SAS table into                                         
      a Python dictionary(list of lists?) and then to iterate over the key/value pairs and                               
      finally create a panda dataframe that I could pass back to SAS.                                                    
      Interfacing with a relational database will be daunting?                                                           
      There is even a HASH table somewhere in this Python code.                                                          
    ;                                                                                                                    
                                                                                                                         
    *              _                 _                                                                                   
      __ _     ___(_)_ __ ___  _ __ | | ___    ___ __ _ ___  ___                                                         
     / _` |   / __| | '_ ` _ \| '_ \| |/ _ \  / __/ _` / __|/ _ \                                                        
    | (_| |_  \__ \ | | | | | | |_) | |  __/ | (_| (_| \__ \  __/                                                        
     \__,_(_) |___/_|_| |_| |_| .__/|_|\___|  \___\__,_|___/\___|                                                        
                              |_|                                                                                        
    ;                                                                                                                    
                                                                                                                         
    INPUT                                                                                                                
    =====                                                                                                                
                                                                                                                         
    %let bus=Hello World pllc.;                                                                                          
                                                                                                                         
    PROCESS                                                                                                              
    =======                                                                                                              
                                                                                                                         
    %utl_submit_py64("                                                                                                   
    import pyperclip;                                                                                                    
    from cleanco import cleanco;                                                                                         
    business_name = '&bus';                                                                                              
    x = cleanco(business_name);                                                                                          
    NAM = x.clean_name();                                                                                                
    TYP = x.type();                                                                                                      
    STRNG=str(NAM) + str(TYP);                                                                                           
    pyperclip.copy(STRNG);                                                                                               
    ",return=frompy);                                                                                                    
                                                                                                                         
    OUTPUT                                                                                                               
    ======                                                                                                               
                                                                                                                         
    %put &=frompy;                                                                                                       
                                                                                                                         
    FROMPY=Hello World['Limited Liability Company', 'Professional Limited Liability Company']                            
                                                                                                                         
    *_        _ _     _            __                                                                                    
    | |__    | (_)___| |_    ___  / _|  _ __   __ _ _ __ ___   ___  ___                                                  
    | '_ \   | | / __| __|  / _ \| |_  | '_ \ / _` | '_ ` _ \ / _ \/ __|                                                 
    | |_) |  | | \__ \ |_  | (_) |  _| | | | | (_| | | | | | |  __/\__ \                                                 
    |_.__(_) |_|_|___/\__|  \___/|_|   |_| |_|\__,_|_| |_| |_|\___||___/                                                 
     _                   _                                                                                               
    (_)_ __  _ __  _   _| |_                                                                                             
    | | '_ \| '_ \| | | | __|                                                                                            
    | | | | | |_) | |_| | |_                                                                                             
    |_|_| |_| .__/ \__,_|\__|                                                                                            
            |_|                                                                                                          
    ;                                                                                                                    
                                                                                                                         
    options validvarname=upcase;                                                                                         
    libname sd1 "d:/sd1";                                                                                                
    data sd1.have;                                                                                                       
      retain recs 0 key value;                                                                                           
      array abv[17] $32 (                                                                                                
           "lp", "llp", "lllp", "llc", "pllc", "corp", "inc","corp.", "inc.",                                            
            "company", "corporation","ltd","ltd.","co","co."                                                             
           "incorporated", "limited"                                                                                     
            );                                                                                                           
       do cmps= "Alcoa", "IBM" ;                                                                                         
           do abvs=1 to dim(abv);                                                                                        
             value=catx(" ",cmps,abv[abvs]);                                                                             
             recs=recs+1;                                                                                                
             key=cats('A',put(recs,z3.));                                                                                
             output;                                                                                                     
          end;                                                                                                           
       end;                                                                                                              
       keep key value;                                                                                                   
    run;quit;                                                                                                            
                                                                                                                         
                                                                                                                         
    SD1.HAVE total obs=34                                                                                                
                                                                                                                         
      KEY     VALUE                                                                                                      
                                                                                                                         
      A001    Alcoa lp                                                                                                   
      A002    Alcoa llp                                                                                                  
      A003    Alcoa lllp                                                                                                 
      A004    Alcoa llc                                                                                                  
      A005    Alcoa pllc                                                                                                 
      A006    Alcoa corp                                                                                                 
      A007    Alcoa inc                                                                                                  
      A008    Alcoa corp.                                                                                                
      A009    Alcoa inc.                                                                                                 
      A010    Alcoa company                                                                                              
      A011    Alcoa corporation                                                                                          
      A012    Alcoa ltd                                                                                                  
      A013    Alcoa ltd.                                                                                                 
      A014    Alcoa co                                                                                                   
      A015    Alcoa co.                                                                                                  
      A016    Alcoa incorporated                                                                                         
      A017    Alcoa limited                                                                                              
      A018    IBM lp                                                                                                     
      A019    IBM llp                                                                                                    
      A020    IBM lllp                                                                                                   
      A021    IBM llc                                                                                                    
      A022    IBM pllc                                                                                                   
      A023    IBM corp                                                                                                   
      A024    IBM inc                                                                                                    
      A025    IBM corp.                                                                                                  
      A026    IBM inc.                                                                                                   
      A027    IBM company                                                                                                
      A028    IBM corporation                                                                                            
      A029    IBM ltd                                                                                                    
      A030    IBM ltd.                                                                                                   
      A031    IBM co                                                                                                     
      A032    IBM co.                                                                                                    
      A033    IBM incorporated                                                                                           
      A034    IBM limited                                                                                                
                                                                                                                         
                                                                                                                         
    *            _               _                                                                                       
      ___  _   _| |_ _ __  _   _| |_                                                                                     
     / _ \| | | | __| '_ \| | | | __|                                                                                    
    | (_) | |_| | |_| |_) | |_| | |_                                                                                     
     \___/ \__,_|\__| .__/ \__,_|\__|                                                                                    
                    |_|                                                                                                  
    ;                                                                                                                    
                                                                                                                         
     WORK.WANT total obs=34                                                                                              
                                                                                                                         
     COMPANY    TYPE_OF_BUSINESS                                                                                         
                                                                                                                         
      Alcoa     Limited Partnership                                                                                      
      Alcoa     Limited Liability Partnership                                                                            
      Alcoa     Limited Liability Limited Partnership                                                                    
      Alcoa     Limited Liability Company                                                                                
      Alcoa     Limited Liability Company                                                                                
      Alcoa     Corporation                                                                                              
      Alcoa     Corporation                                                                                              
      Alcoa     Corporation                                                                                              
      Alcoa     Corporation                                                                                              
      Alcoa     Corporation                                                                                              
      Alcoa     Corporation                                                                                              
      Alcoa     Limited                                                                                                  
      Alcoa     Limited                                                                                                  
      Alcoa     Corporation                                                                                              
      Alcoa     Corporation                                                                                              
      Alcoa     Corporation                                                                                              
      Alcoa     Limited                                                                                                  
      IBM       Limited Partnership                                                                                      
      IBM       Limited Liability Partnership                                                                            
      IBM       Limited Liability Limited Partnership                                                                    
      IBM       Limited Liability Company                                                                                
      IBM       Limited Liability Company                                                                                
      IBM       Corporation                                                                                              
      IBM       Corporation                                                                                              
      IBM       Corporation                                                                                              
      IBM       Corporation                                                                                              
      IBM       Corporation                                                                                              
      IBM       Corporation                                                                                              
      IBM       Limited                                                                                                  
      IBM       Limited                                                                                                  
      IBM       Corporation                                                                                              
      IBM       Corporation                                                                                              
      IBM       Corporation                                                                                              
      IBM       Limited                                                                                                  
                                                                                                                         
    *                                                                                                                    
     _ __  _ __ ___   ___ ___  ___ ___                                                                                   
    | '_ \| '__/ _ \ / __/ _ \/ __/ __|                                                                                  
    | |_) | | | (_) | (_|  __/\__ \__ \                                                                                  
    | .__/|_|  \___/ \___\___||___/___/                                                                                  
    |_|                                                                                                                  
    ;                                                                                                                    
                                                                                                                         
    %utlfkil(d:/xpt/stor.xpt);                                                                                           
                                                                                                                         
    proc datasets lib=work;                                                                                              
     delete want;                                                                                                        
    run;quit;                                                                                                            
                                                                                                                         
    %utl_submit_py64_37("                                                                                                
    import pandas as pd;                                                                                                 
    import numpy as np;                                                                                                  
    import pyreadstat;                                                                                                   
    import itertools;                                                                                                    
    from csv import reader;                                                                                              
    from cleanco import cleanco;                                                                                         
    basic_cleanup, meta = pyreadstat.read_sas7bdat('d:/sd1/have.sas7bdat',                                               
      catalog_file='d:/fmt/formats.sas7bcat', formats_as_category=True);                                                 
    dict(sorted(basic_cleanup.values.tolist()));                                                                         
    from collections import OrderedDict;                                                                                 
    basic_cleanup_tests=OrderedDict(basic_cleanup.values.tolist());                                                      
    print(basic_cleanup_tests);                                                                                          
    stor=pd.DataFrame();                                                                                                 
    for testname, variation in basic_cleanup_tests.items():;                                                             
    .     tmps = cleanco(variation);                                                                                     
    .     NAME=tmps.clean_name();                                                                                        
    .     TYPE=tmps.type();                                                                                              
    .     STRNG=str(NAME) + str(TYPE);                                                                                   
    .     tmp= df=pd.DataFrame(list(reader([STRNG])));                                                                   
    .     stor = stor.append(tmp, ignore_index=True);                                                                    
    stor.columns = ['BUSINESS', 'delete'];                                                                               
    stor=pd.DataFrame(stor['BUSINESS']);                                                                                 
    print(stor);                                                                                                         
    pyreadstat.write_xport(stor,'d:/xpt/tror.xpt',table_name='stor');                                                    
    ");                                                                                                                  
                                                                                                                         
    libname xpt xport "d:/xpt/stor.xpt";                                                                                 
    proc contents data=xpt._all_;                                                                                        
    run;quit;                                                                                                            
    data want;                                                                                                           
      set xpt.stor;                                                                                                      
    run;quit;                                                                                                            
    libname xpt clear;                                                                                                   
                                                                                                                         
                                                                                                                         
    *              _                                                                                                     
     _ __  _   _  | | ___   __ _                                                                                         
    | '_ \| | | | | |/ _ \ / _` |                                                                                        
    | |_) | |_| | | | (_) | (_| |                                                                                        
    | .__/ \__, | |_|\___/ \__, |                                                                                        
    |_|    |___/           |___/                                                                                         
    ;                                                                                                                    
                                              BUSINESS                                                                   
                                                                                                                         
    0                     Alcoa['Limited Partnership']                                                                   
    1           Alcoa['Limited Liability Partnership']                                                                   
    2   Alcoa['Limited Liability Limited Partnership']                                                                   
    3               Alcoa['Limited Liability Company']                                                                   
    4                Alcoa['Limited Liability Company'                                                                   
    5                             Alcoa['Corporation']                                                                   
    6                             Alcoa['Corporation']                                                                   
    7                             Alcoa['Corporation']                                                                   
    8                             Alcoa['Corporation']                                                                   
    9                             Alcoa['Corporation']                                                                   
    10                            Alcoa['Corporation']                                                                   
    11                                Alcoa['Limited']                                                                   
    12                                Alcoa['Limited']                                                                   
    13                            Alcoa['Corporation']                                                                   
    14                            Alcoa['Corporation']                                                                   
    15                            Alcoa['Corporation']                                                                   
    16                                Alcoa['Limited']                                                                   
    17                      IBM['Limited Partnership']                                                                   
    18            IBM['Limited Liability Partnership']                                                                   
    19    IBM['Limited Liability Limited Partnership']                                                                   
    20                IBM['Limited Liability Company']                                                                   
    21                 IBM['Limited Liability Company'                                                                   
    22                              IBM['Corporation']                                                                   
    23                              IBM['Corporation']                                                                   
    24                              IBM['Corporation']                                                                   
    25                              IBM['Corporation']                                                                   
    26                              IBM['Corporation']                                                                   
    27                              IBM['Corporation']                                                                   
    28                                  IBM['Limited']                                                                   
    29                                  IBM['Limited']                                                                   
    30                              IBM['Corporation']                                                                   
    31                              IBM['Corporation']                                                                   
    32                              IBM['Corporation']                                                                   
    33                                  IBM['Limited']                                                                   
                                                                                                                         
                                                                                                                         
