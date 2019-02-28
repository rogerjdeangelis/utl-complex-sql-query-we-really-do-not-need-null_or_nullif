# utl-complex-sql-query-we-really-do-not-need-null_or_nullif
Complex sql query we really do not need nulls or nullif. 
    Complex sql query we really do not need nulls or nullif.                                                  
                                                                                                              
    I think code is more readable without a null function?                                                    
    The mnay forms f nulls in R is a problem?                                                                 
    Less is more, but 128bit floats would be nice.                                                            
                                                                                                              
    github                                                                                                    
    https://tinyurl.com/y5x48jaj                                                                              
    https://github.com/rogerjdeangelis/utl-complex-sql-query-we-really-do-not-need-null_or_nullif             
                                                                                                              
    https://tinyurl.com/y68df6sl                                                                              
    https://stackoverflow.com/questions/54876075/nullif-statement-in-sas                                      
                                                                                                              
    Richard                                                                                                   
    https://stackoverflow.com/users/1249962/richard                                                           
                                                                                                              
                                                                                                              
    =====                                                                                                     
    INPUT                                                                                                     
    =====                                                                                                     
                                                                                                              
    data have;                                                                                                
     input Id Value$ @@;                                                                                      
    cards4;                                                                                                   
    1 A 1 A 1 B 2 @ 2 @ 2 @ 3 A 3 A 3 A 3 A 3 @ 4 B 4 B 4 B                                                   
    ;;;;                                                                                                      
    run;quit;                                                                                                 
                                                                                                              
    RULES                                                                                                     
                                                                                                              
    WORK.HAVE total obs=14                                                                                    
                                                                                                              
                 |  RULES(with output)                                                                        
                 |                                                                                            
      ID   VAL   |  ID   VAL                                                                                  
                 |                                                                                            
                 |                                                                                            
       1    A    |   1   @    If 2 or more distinct values                                                    
       1    A    |            ignoring @ then set to @                                                        
       1    B    |                                                                                            
                 |                                                                                            
       2    @    |   2   @    if all values are @                                                             
       2    @    |            then output onre record                                                         
       2    @    |                                                                                            
                 |                                                                                            
       3    A    |   3   A    if all values the same                                                          
       3    A    |            ignoring @ then set to                                                          
       3    A    |            that value 'A'                                                                  
       3    A    |                                                                                            
       3    @    |                                                                                            
                 |                                                                                            
       4    B    |   4   B     if all values the same                                                         
       4    B    |            ignoring @ then set                                                             
       4    B    |            to that vale 'B'                                                                
                                                                                                              
    ======                                                                                                    
    OUTPUT                                                                                                    
    ======                                                                                                    
                                                                                                              
     WORK.WANT total obs=4                                                                                    
                                                                                                              
      ID    VALUE                                                                                             
                                                                                                              
       1      @                                                                                               
       2      @                                                                                               
       3      A                                                                                               
       4      B                                                                                               
                                                                                                              
    ========                                                                                                  
    SOLUTION                                                                                                  
    ========                                                                                                  
                                                                                                              
    proc sql;                                                                                                 
       create table want as                                                                                   
       select id,                                                                                             
         case                                                                                                 
           when count(                                                                                        
              distinct                                                                                        
              case                                                                                            
                when value ne '@' then value                                                                  
              end                                                                                             
              ) = 1 then max(value)                                                                           
           else '@'                                                                                           
         end as value                                                                                         
       from have                                                                                              
       group by id                                                                                            
    ;quit;                                                                                                    
                                                                                                              
