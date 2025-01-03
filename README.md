# Fraud-detection-by-sql
/*
1.What is the transaction id , user id , amount ,type
,location, is fraud or not of highest amount
2.number of fraud transition city wise 
3.Most number of type of transaction
4.location wise highest amount
5.Rank top 10 highest amount and their transaction type
6.highest amount that is not fraud amount
*/
create database Fraud_dection_db;
use Fraud_dection_db;

select*from fraud_dection;

select Transaction_id,user_id,transaction_amount,transaction_type,location,is_fraud
from fraud_dection
order by transaction_amount desc
limit 1;

select location,count(is_fraud) as fraud_count
from fraud_dection
where is_fraud = 1
group by location;

select transaction_type,count(transaction_type) as numb_type
from fraud_dection
group by transaction_type;

select location,max(transaction_amount) as Highest_per_location
from fraud_dection
group by location;

select transaction_amount,transaction_type,
dense_rank() over (partition by transaction_type order by transaction_amount desc) as Highrank
from fraud_dection
limit 10;

select max(transaction_amount)
from fraud_dection
where is_fraud = 0;

/*
Find the total transaction amount for all fraudulent transactions.

Determine the locations with the highest number of fraudulent transactions.

Analyze the trend of fraudulent transactions per month.

Retrieve high-value transactions (over 1,000,000) in New York and Paris.

Identify users with the highest number of transactions and their total transaction amounts.

Determine which transaction types are most associated with fraudulent activities.

Identify the periods with the highest occurrence of fraud.
*/
select sum(transaction_amount) as fraudAmount
from fraud_dection
where is_fraud = 1;

select location,max(transaction_amount) as HighFraudAmount
from fraud_dection
where is_fraud = 1
group by location
order by HighFraudAmount desc;

select DATE_FORMAT(STR_TO_DATE(Timestamp, '%m/%d/%Y'), '%Y-%m') AS Fraud_Month, COUNT(*) AS Fraud_Count
 FROM fraud_dection
 WHERE Is_Fraud = 1 
 GROUP BY Fraud_Month
 ORDER BY Fraud_Month;
 
 select *
 from fraud_dection
 where transaction_amount > 1000000 and location in ("Paris","New York");
 
 select user_id,count(*) as total_transaction , sum(transaction_amount) as total_amount
 from fraud_dection
 group by user_id
 order by total_transaction desc;
 
 select transaction_type, count(transaction_type) as fraud_type
 from fraud_dection
 where is_fraud = 1
 group by transaction_type
 order by fraud_type desc;
 
SELECT DATE_FORMAT(STR_TO_DATE(Timestamp, '%m/%d/%Y %H:%i:%s'), '%m') AS Fraud_Hour, 
       COUNT(*) AS Fraud_Count
FROM fraud_dection
WHERE Is_Fraud = 1
GROUP BY Fraud_Hour
ORDER BY Fraud_Count DESC;



