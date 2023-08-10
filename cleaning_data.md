What issues will you address by cleaning the data?
The issues that I had addressed by cleaning data includes: (incorrect, corrupted, incorrectly formatted, duplicate, or incomplete data). 


Queries:
Below, provide the SQL queries you used to clean your data.
1. Sets the Currency code fro each country as per its country name. Also, update/Change the currency code for those who have country name but wrong curency code like Canada has USD after updation it will have CAD.

Query:
 update allsessions
set currencycode = (case 
when country = 'Canada'  and (currencycode is null or currencycode = 'USD') then 'CAD'
when country = 'Turkey'  and (currencycode is null or currencycode = 'USD') then 'TRY'
when country = 'Czechia'  and (currencycode is null or currencycode = 'USD') then 'CZK'
when country = 'Hong Kong'  and (currencycode is null or currencycode = 'USD') then 'HKD'
when (country = 'Spain' or country = 'Germany' or country = 'Belgium' 
	  or country = 'Finland' or country = 'France' or country = 'Ireland'
	 or country = 'Italy' or country = 'Netherlands') and (currencycode is null or currencycode = 'USD') then 'EUR'
when country = 'Argentina'  and (currencycode is null or currencycode = 'USD') then 'ARS'
when country = 'Australia'  and (currencycode is null or currencycode = 'USD') then 'AUD'
when country = 'Belgium'  and (currencycode is null or currencycode = 'USD') then 'ARS'
when country = 'Brazil'  and (currencycode is null or currencycode = 'USD') then 'BRL'
when country = 'Chile'  and (currencycode is null or currencycode = 'USD') then 'CLF'
when country = 'Croatia'  and (currencycode is null or currencycode = 'USD') then 'HRK'
when country = 'Denmark'  and (currencycode is null or currencycode = 'USD') then 'DKK'
when country = 'El Salvador'  and (currencycode is null or currencycode = 'USD') then 'SVC'
when country = 'Hungary'  and (currencycode is null or currencycode = 'USD') then 'HUF'
when country = 'India'  and (currencycode is null or currencycode = 'USD') then 'INR'
when country = 'Indonesia'  and (currencycode is null or currencycode = 'USD') then 'IDR'
when country = 'Japan'  and (currencycode is null or currencycode = 'USD') then 'JPY'
when country = 'Mexico'  and (currencycode is null or currencycode = 'USD') then 'MXN'
when country = 'Myanmar'  and (currencycode is null or currencycode = 'USD') then 'MMK'
when country = 'Norway'  and (currencycode is null or currencycode = 'USD') then 'NOK'
when country = 'Pakistan'  and (currencycode is null or currencycode = 'USD') then 'PKR'
when country = 'Peru'  and (currencycode is null or currencycode = 'USD') then 'PEN'
when country = 'Phillippines'  and (currencycode is null or currencycode = 'USD') then 'PHP'
when country = 'Romania'  and (currencycode is null or currencycode = 'USD') then 'RON'
when country = 'Philippines'  and (currencycode is null or currencycode = 'USD') then 'PHP'
when country = 'Russia'  and (currencycode is null or currencycode = 'USD') then 'RUB'
when country = 'Singapore'  and (currencycode is null or currencycode = 'USD') then 'SGD'
when country = 'Singapore'  and (currencycode is null or currencycode = 'USD') then 'SGD'
when country = 'Sweden'  and (currencycode is null or currencycode = 'USD') then 'SEK'
when country = 'Switzerland'  and (currencycode is null or currencycode = 'USD') then 'CHE'
when country = 'Taiwan'  and (currencycode is null or currencycode = 'USD') then 'TWD'
when country = 'Thailand'  and (currencycode is null or currencycode = 'USD') then 'THB'
when country = 'Ukraine'  and (currencycode is null or currencycode = 'USD') then 'UAH'
when country = 'United Kingdom'  and (currencycode is null or currencycode = 'USD') then 'GBP'
when country = 'Vietnam'  and (currencycode is null or currencycode = 'USD') then 'VND'
when country = 'New Zealand'  and (currencycode is null or currencycode = 'USD') then 'NZD'
when country = 'United States' then 'USD' end);

2. Update totaltransactionrevenueod of allsession column as 0 whose value is null in order make data as readable form.

Query: update allsessions
set totaltransactionrevenue = (
	CASE 
		when totaltransactionrevenue is not null then totaltransactionrevenue
		when totaltransactionrevenue is null then 0 end
);

3. Update the city value as N/A in those row who have 'not available in demo dataset' or  '(not set)' so that data becomes more consistent.

Query: 
update allsessions
set city = (
    CASE 
        WHEN city = 'not available in demo dataset' or city = '(not set)' THEN 'N/A' 
        when city is not null then city end
    );

4. In Analytics table, have alot of values that are repeated with same input but the only difference is unit_price;

Query: 
Select * from analytics -- results into 4301122 data

CREATE TABLE analytics_2 AS
SELECT DISTINCT *
FROM analytics;
select * from analytics_2 -- results into 1739308 data

5. Remove irrelevant, redundant, or duplicate data:
ALTER TABLE analytics ALTER COLUMN timeonsite TYPE VARCHAR USING LPAD(timeonsite::VARCHAR, 6, '0');

UPDATE analytics SET timeonsite = CONCAT( SUBSTRING(timeonsite, 1, 2), ':', SUBSTRING(timeonsite, 3, 2), ':', SUBSTRING(timeonsite, 5, 2) );

UPDATE analytics SET timeonsite = TIME '00:00:00' + INTERVAL '1 hour' * CAST(SUBSTR(timeonsite, 1, 2) AS INTEGER) + INTERVAL '1 minute' * CAST(SUBSTR(timeonsite, 4, 2) AS INTEGER) + INTERVAL '1 second' * CAST(SUBSTR(timeonsite, 7, 2) AS INTEGER);

ALTER TABLE analytics ALTER COLUMN timeonsite TYPE TIME USING timeonsite::TIME;

