# sql-data-cleaning-project
This SQL data cleaning project demonstrates basic to advanced SQL data cleaning techniques which will be used on a cafe dataset. 
It focuses on finding duplicates, missing values, invalid entries and inconsistent formating.

TOOLS USED : MS SQL SERVER

DATASET : CAFE SALES DATASET

Cleaning Tasks Performed:
- Replaced NULLs using ISNULL
- Removed duplicate rows 
- Standardized date formats
- Trimmed whitespace using LTRIM, RTRIM

# SQL QUERIES

select * from DirtyData

SELECT COUNT(*) FROM DirtyData

-- RENAMING COLUMN NAMES
EXEC sp_rename 'DirtyData.Price Per Unit', 'per_unit_price', 'COLUMN';
EXEC sp_rename 'DirtyData.Total Spent', 'TotalAmt', 'COLUMN';
EXEC sp_rename 'DirtyData.Transaction ID', 'TransactionID', 'COLUMN';
EXEC sp_rename 'DirtyData.Payment Method', 'PaymentMethod', 'COLUMN';
EXEC sp_rename 'DirtyData.Transaction Date', 'TransactionDate', 'COLUMN';


-- EXPLORING THE DATASET
select Item, per_unit_price ,count(Item) as counts from DirtyData
group by Item, per_unit_price
order by Item


-- find out perunitprice of each item
select Item, per_unit_price from DirtyData
group by Item , per_unit_price
order by Item


-- FILLING NULL VALLUES

-- filling the null values in perunitprice column - cake
UPDATE DirtyData
SET per_unit_price = 3
WHERE Item = 'Cake' AND per_unit_price IS NULL;

-- filling the null values in perunitprice column - coffee
-UPDATE DirtyData
-SET per_unit_price = 2
-WHERE Item = 'Coffee' AND per_unit_price IS NULL;

-- filling the null values in perunitprice column - cookie
-UPDATE DirtyData
-SET per_unit_price = 1
-WHERE Item = 'Cookie' AND per_unit_price IS NULL;

-- filling the null values in perunitprice column - salad
UPDATE DirtyData
SET per_unit_price = 5
WHERE Item = 'Salad' AND per_unit_price IS NULL;

-- filling the null values in perunitprice column - sandwich
UPDATE DirtyData
SET per_unit_price = 4
WHERE Item = 'Sandwich' AND per_unit_price IS NULL;

-- filling the null values in perunitprice column - smoothie
UPDATE DirtyData
SET per_unit_price =4
WHERE Item = 'Smoothie' AND per_unit_price IS NULL;

-- filling the null values in perunitprice column - juice
UPDATE DirtyData
SET per_unit_price = 3
WHERE Item = 'Juice' AND per_unit_price IS NULL;

-- filling the null values in perunitprice column - tea
UPDATE DirtyData
SET per_unit_price = 1.5
WHERE Item = 'Tea' AND per_unit_price IS NULL;




-- REPLACING NULL VALUES IN ITEM COLUMB WITH RESPECT TO THEIR PER UNIT PRICES

UPDATE DirtyData set Item = 'Cookie'
WHERE per_unit_price = 1 and Item is null; 

UPDATE DirtyData set Item = 'Coffee'
WHERE per_unit_price = 2 and Item is null;

UPDATE DirtyData set Item = 'Cake'
WHERE per_unit_price = 3 and Item is null;

UPDATE DirtyData set Item = 'Sandwich'
WHERE per_unit_price = 4 and Item is null;

UPDATE DirtyData set Item = 'Tea'
WHERE per_unit_price = 1.5 and Item is null;

UPDATE DirtyData set Item = 'Salad'
WHERE per_unit_price = 5 and Item is null;

-- DELETE ROWS CONTAINING ITEM AS NULL
delete from DirtyData where Item is null;




-- REPLACING ERROR VALUES IN ITEM WITH RESPECT TO THEIR PER UNIT PRICE

UPDATE DirtyData set Item = 'Cookie'
WHERE per_unit_price = 1 and Item = 'ERROR';

UPDATE DirtyData set Item = 'Coffee'
WHERE per_unit_price = 2 and Item = 'ERROR'

UPDATE DirtyData set Item = 'Cake'
WHERE per_unit_price = 3 and Item = 'ERROR'

UPDATE DirtyData set Item = 'Smoothie'
WHERE per_unit_price = 4 and Item = 'ERROR'

UPDATE DirtyData set Item = 'Salad'
WHERE per_unit_price = 5 and Item = 'ERROR'

UPDATE DirtyData set Item = 'Tea'
WHERE per_unit_price = 1.5 and Item = 'ERROR'

-- DELETE ROWS CONTAINING ITEM AS ERROR
delete from DirtyData where Item = 'ERROR';




-- REPLACING UNKNOWN VALUES IN ITEM WITH RESPECT TO THEIR PER UNIT PRICE
UPDATE DirtyData set Item = 'Cookie'
WHERE per_unit_price = 1 and Item = 'UNKNOWN'

UPDATE DirtyData set Item = 'Coffee'
WHERE per_unit_price = 2 and Item = 'UNKNOWN'

UPDATE DirtyData set Item = 'Cake'
WHERE per_unit_price = 3 and Item = 'UNKNOWN'

UPDATE DirtyData set Item = 'Sandwich'
WHERE per_unit_price = 4 and Item = 'UNKNOWN'

UPDATE DirtyData set Item = 'Salad'
WHERE per_unit_price = 5 and Item = 'UNKNOWN'

UPDATE DirtyData set Item = 'Tea'
WHERE per_unit_price = 1.5 and Item = 'UNKNOWN'

-- DELETE ROWS CONTAINING ITEM AS UNKNOWN
delete from DirtyData where Item = 'UNKNOWN';




-- TOTAL ROWS CONTAINING NULL VALUES/UNKNOWN/ERROR
select count(*) AS NullCounts from DirtyData where Item is null
select count(*) AS ErrorCounts from DirtyData where Item = 'ERROR'
select count(*) AS UnknownCounts from DirtyData where Item = 'UNKNOWN'



-- find distinct PaymentMethod
select PaymentMethod , count(*) from DirtyData
group by PaymentMethod
-- delete rows containing PaymentMethod as null
delete from DirtyData where PaymentMethod is null


-- find distinct locations
select Location, count (*) from DirtyData 
group by Location

-- delete rows containing Location as null
delete from DirtyData where Location is null


-- DELETE ROWS IN TRANSACTIONDATE WITH NULL VALUES
DELETE FROM DirtyData WHERE TransactionDate IS NULL;


SELECT * FROM DirtyData 

-- ONCE AGAIN CHECK NULL VALUES IN EACH COLUMN
SELECT * FROM DirtyData WHERE PaymentMethod IS NULL;
SELECT * FROM DirtyData WHERE TransactionDate IS NULL;
SELECT * FROM DirtyData WHERE Location IS NULL;
SELECT * FROM DirtyData WHERE Item IS NULL;
SELECT * FROM DirtyData WHERE TransactionID IS NULL;

-- Now, only numerical values contains null values
select distinct Quantity from DirtyData


-- UPDATE QUANTITY USING THE FORMULA : Quantity = TotalAmt/ per_unit_price
update DirtyData set Quantity = TotalAmt/ per_unit_price
where Quantity is null;


-- UPDATE TOTALAMT USING THE FORMULA : TotalAmt = Quantity * per_unit_price
update DirtyData set TotalAmt = Quantity * per_unit_price
where TotalAmt is null;

-- ONCE AGAIN CHECK FOR NULL VALUES
SELECT * FROM DirtyData WHERE TotalAmt IS NULL;
SELECT * FROM DirtyData WHERE Quantity IS NULL;

-- NOW DELETE ROWS HAVING BOTH QTY AND TOTALAMT AS NULL, AS IT IS NOT USEFUL
DELETE FROM DirtyData WHERE Quantity IS NULL AND TotalAmt IS NULL;


-- -- Remove leading/trailing whitespaces
UPDATE DirtyData
SET Item = LTRIM(RTRIM(Item));


-- STANDARDIZE DATE COLUMN 
ALTER TABLE DirtyData
ADD CleanTransactionDate DATE;

UPDATE DirtyData
SET CleanTransactionDate = CAST(TransactionDate AS DATE)
WHERE ISDATE(TransactionDate) = 1;

ALTER TABLE DirtyData
DROP COLUMN TransactionDate;

EXEC sp_rename 'DirtyData.CleanTransactionDate', 'TransactionDate', 'COLUMN';







