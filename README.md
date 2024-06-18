# CovidDeath excel file
-- 1. But before we started separation we did quick data processing to upload data into Postgresql
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
