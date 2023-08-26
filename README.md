# CriminalJusticeDB

Create table Aliases 
CREATE TABLE Aliases ( 
Alias_iD NUMERIC(10), 
Criminal_ID NUMERIC, 
Alias VARCHAR(10) 
); 
 
-- Create table Criminals 
CREATE TABLE Criminals ( 
Criminal_ID NUMERIC, 
Last VARCHAR(15), 
First VARCHAR(10), 
Street VARCHAR(30), 
City VARCHAR(20), 
Zip CHAR(5), 
Phone CHAR(10), 
V_status CHAR(1), 
P_status CHAR(1) 
); 
 
-- Create table Crimes 
CREATE TABLE Crimes ( 
Crime_ID NUMERIC, 
Criminal_ID NUMERIC, 
Classification CHAR(2), 
Status CHAR(2), 
Hearing_date DATE, 
Appeal_cut_date DATE 
); 
 
-- Create table Sentences 
CREATE TABLE Sentences ( 
Sentence_ID NUMERIC, 
Criminal_ID NUMERIC, 
Prob_ID NUMERIC, 
Violations NUMERIC, 
Type CHAR(1), 
Start_date DATE, 
End_date DATE 
); 
 
-- Create table Prob_officers 
CREATE TABLE Prob_officers ( 
Prob_ID NUMERIC, 
Last VARCHAR(15), 
First VARCHAR(10), 
Street VARCHAR(30), 
City NUMERIC, 
State CHAR(2), 
Zip CHAR(5), 
Phone CHAR(10), 
Email VARCHAR(30), 
Status CHAR(1) 
); 
 
-- Create table Crime_charges 
CREATE TABLE Crime_charges ( 
Charge_ID NUMERIC, 
Crime_ID NUMERIC, 
Crime_code NUMERIC, 
Charge_status CHAR(7), 
Fine_amount DECIMAL(2), 
Court_fee DECIMAL(2), 
Amount_paid DECIMAL(2), 
Pay_due_date DATE 
); 
 
-- Create table Crime_officers 
CREATE TABLE Crime_officers ( 
Crime_ID NUMERIC, 
Officer_ID NUMERIC 
); 
 
-- Create table Officers 
CREATE TABLE Officers ( 
Officer_ID NUMERIC, 
Last VARCHAR(15), 
First VARCHAR(10), 
Badge VARCHAR(14), 
Precinct CHAR(4), 
Phone CHAR(10), 
Status CHAR(1) 
); 
 
-- Create table Appeals 
CREATE TABLE Appeals ( 
Appeal_ID NUMERIC, 
Crime_ID NUMERIC, 
Filing_date DATE, 
Hearing_date DATE, 
Status CHAR(1) 
); 
 
-- Create table Crime_codes 
CREATE TABLE Crime_codes ( 
Crime_code NUMERIC, 
Code_description VARCHAR(30) 
); 
 
 
 
-- Add default value 'U' to the Classification column in the Crimes table 
ALTER TABLE Crimes 
ALTER COLUMN Classification CHAR(2) NOT NULL DEFAULT 'U'; 
 
 
-- Step 1: Add a new nullable column with default value 
ALTER TABLE Crimes 
ADD Classification_New CHAR(2) NULL; 
 
-- Step 2: Update existing rows to set the default value 
UPDATE Crimes 
SET Classification_New = ISNULL(Classification, 'U'); 
 
-- Step 3: Drop the original Classification column 
ALTER TABLE Crimes 
DROP COLUMN Classification; 
 
-- Step 4: Rename the new column to Classification 
EXEC sp_rename 'Crimes.Classification_New', 'Classification', 'COLUMN'; 
 
-- Add a column named Date_Recorded to the Crimes table 
ALTER TABLE Crimes 
ADD Date_Recorded DATE DEFAULT GETDATE(); 
 
 
-- Add a column named Pager# to the Prob_officers table 
ALTER TABLE Prob_officers 
ADD [Pager#] VARCHAR(15); 
 
-- Change the Alias column in the Aliases table to accommodate up to 20 characters 
ALTER TABLE Aliases 
ALTER COLUMN Alias VARCHAR(20); 
