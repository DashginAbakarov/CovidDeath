# CovidDeath excel file

###NOTA BENE !!! I have changed name of 2 columns in excel sheet. 1) location to country, 2) date to date_column

-- 1. But before we started separation we did quick data processing before upload data into Postgresql

-- a) Our COVID data column values have many digits after the decimal point (e.g., 0.09383617), which will consume a lot of storage in PostgreSQL. To save space, we should round all values to three decimal places.

-- b) We need to update our "date_column" to use the "yyyy-mm-dd" format, which is the required SQL DATE format.

-- 2. We separated the original COVID death and vaccination Excel file into two files. First, we removed all columns related to vaccination and saved the file with only death-related columns under the name "CovidDeath." Then, we removed the death-related columns from the original file and saved it with only vaccination-related columns.

-- 3. The next step is to find the maximum value for each column. This will help us choose the appropriate data type and length when creating the PostgreSQL table.

  a) iso_code column as it contains text value we use  =MAX(LEN(CovidDeaths!A:A)) 

  b) continent column =MAX(LEN(CovidDeaths!B:B)) 

  c) country ==MAX(LEN(CovidDeaths!C:C)) 

  d) population =MAX(LEN(CovidDeaths!E:E)) 
  
  e) rest of the column contains only numeric value therefore we use MAX(array_of _column) 

-- 4. there can be a question, how to find the text value with the maximum character length in a column. If we use MAX(LEN(COUNTRY_column)), we get the length (e.g., 13). For example, "North_America" has 13 characters. To retrieve the actual text value, not just the length, we can use a combination of the INDEX and MATCH functions.

  a) The MATCH function, MATCH(lookup_value, lookup_array, match_type), helps find the exact position of a value in an array(can be column or row). Here's the syntax with your specific requirements:
  
  -lookup_value: MAX(LEN(country_column)) (to find the maximum length value)

  -lookup_array: country_column (the array of the country column)

  -match_type: 0 (to find an exact match)
  
  exact formula is =MATCH(MAX(LEN(CovidDeaths_csv!B:B)),LEN(CovidDeaths_csv!B:B),0), 

  b) To find the value using the INDEX function,INDEX(array, row_num, [column_num]), we need the array, row number and column number.
  
  - array is easy we choose whole country column
  
  - we have already used MATCH function above to find row number. So we will add it into the INDEX function directly.

  - column number is optional you do not need to specify.

  exact formula is =INDEX(CovidDeaths_csv!B:B,MATCH(MAX(LEN(CovidDeaths_csv!B:B)), LEN(CovidDeaths_csv!B:B),0),1). Result will be North America
  
