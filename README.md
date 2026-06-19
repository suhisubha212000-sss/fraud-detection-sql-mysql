-- ============================================
-- FRAUD DETECTION USING SQL ANALYSIS (MySQL)
-- ============================================

-- STEP 1: CREATE DATABASE AND TABLE
DROP DATABASE IF EXISTS fraud_detection;
CREATE DATABASE fraud_detection;
USE fraud_detection;

CREATE TABLE transactions (
    transaction_id VARCHAR(20) PRIMARY KEY,
    customer_id VARCHAR(20),
    amount DECIMAL(10,2),
    location VARCHAR(50),
    merchant_category VARCHAR(50),
    transaction_time DATETIME,
    is_fraud TINYINT(1)
);

-- STEP 2: INSERT SAMPLE DATA (60 transactions, 20 customers)
INSERT INTO transactions VALUES
('TXN00001','CUST001',1250.50,'Bangalore','Grocery','2026-05-01 09:15:00',0),
('TXN00002','CUST001',3400.00,'Bangalore','Electronics','2026-05-02 14:30:00',0),
('TXN00003','CUST001',45000.00,'Mumbai','Online Shopping','2026-05-03 02:10:00',1),
('TXN00004','CUST002',890.00,'Delhi','Fuel','2026-05-01 08:00:00',0),
('TXN00005','CUST002',2100.75,'Delhi','Restaurant','2026-05-02 19:45:00',0),
('TXN00006','CUST002',62000.00,'Chennai','ATM Withdrawal','2026-05-04 03:20:00',1),
('TXN00007','CUST003',1500.00,'Hyderabad','Grocery','2026-05-01 10:00:00',0),
('TXN00008','CUST003',3200.00,'Hyderabad','Travel','2026-05-03 16:00:00',0),
('TXN00009','CUST003',780.50,'Pune','Restaurant','2026-05-05 20:15:00',0),
('TXN00010','CUST004',5200.00,'Bangalore','Electronics','2026-05-02 11:30:00',0),
('TXN00011','CUST004',38000.00,'Kolkata','Online Shopping','2026-05-06 01:45:00',1),
('TXN00012','CUST004',920.00,'Bangalore','Grocery','2026-05-07 09:00:00',0),
('TXN00013','CUST005',1800.00,'Mumbai','Fuel','2026-05-02 17:20:00',0),
('TXN00014','CUST005',2500.00,'Mumbai','Restaurant','2026-05-04 20:00:00',0),
('TXN00015','CUST005',71000.00,'Delhi','ATM Withdrawal','2026-05-08 04:10:00',1),
('TXN00016','CUST006',650.00,'Chennai','Grocery','2026-05-01 12:00:00',0),
('TXN00017','CUST006',1200.00,'Chennai','Fuel','2026-05-03 08:45:00',0),
('TXN00018','CUST006',3100.00,'Hyderabad','Travel','2026-05-05 15:30:00',0),
('TXN00019','CUST007',890.00,'Pune','Restaurant','2026-05-02 19:00:00',0),
('TXN00020','CUST007',55000.00,'Bangalore','Online Shopping','2026-05-06 02:30:00',1),
('TXN00021','CUST007',1450.00,'Pune','Grocery','2026-05-07 10:15:00',0),
('TXN00022','CUST008',2300.00,'Kolkata','Electronics','2026-05-01 13:00:00',0),
('TXN00023','CUST008',4100.00,'Kolkata','Travel','2026-05-04 11:00:00',0),
('TXN00024','CUST008',800.00,'Mumbai','Restaurant','2026-05-06 21:00:00',0),
('TXN00025','CUST009',1950.00,'Delhi','Fuel','2026-05-02 07:30:00',0),
('TXN00026','CUST009',48000.00,'Chennai','ATM Withdrawal','2026-05-05 03:00:00',1),
('TXN00027','CUST009',670.00,'Delhi','Grocery','2026-05-07 09:45:00',0),
('TXN00028','CUST010',3300.00,'Hyderabad','Electronics','2026-05-01 16:20:00',0),
('TXN00029','CUST010',1100.00,'Hyderabad','Restaurant','2026-05-03 19:30:00',0),
('TXN00030','CUST010',2750.00,'Pune','Travel','2026-05-05 14:00:00',0),
('TXN00031','CUST011',920.00,'Bangalore','Grocery','2026-05-02 10:30:00',0),
('TXN00032','CUST011',67000.00,'Mumbai','Online Shopping','2026-05-06 01:15:00',1),
('TXN00033','CUST011',1600.00,'Bangalore','Fuel','2026-05-08 08:00:00',0),
('TXN00034','CUST012',2900.00,'Chennai','Restaurant','2026-05-01 20:00:00',0),
('TXN00035','CUST012',3700.00,'Chennai','Electronics','2026-05-04 15:45:00',0),
('TXN00036','CUST012',850.00,'Kolkata','Grocery','2026-05-06 09:00:00',0),
('TXN00037','CUST013',1300.00,'Delhi','Fuel','2026-05-02 08:15:00',0),
('TXN00038','CUST013',39000.00,'Hyderabad','ATM Withdrawal','2026-05-05 02:45:00',1),
('TXN00039','CUST013',2100.00,'Delhi','Travel','2026-05-07 13:30:00',0),
('TXN00040','CUST014',780.00,'Pune','Restaurant','2026-05-01 19:15:00',0),
('TXN00041','CUST014',1450.00,'Pune','Grocery','2026-05-03 11:00:00',0),
('TXN00042','CUST014',3200.00,'Mumbai','Travel','2026-05-06 16:30:00',0),
('TXN00043','CUST015',2600.00,'Bangalore','Electronics','2026-05-02 12:45:00',0),
('TXN00044','CUST015',58000.00,'Chennai','Online Shopping','2026-05-07 03:20:00',1),
('TXN00045','CUST015',940.00,'Bangalore','Grocery','2026-05-08 10:00:00',0),
('TXN00046','CUST016',1750.00,'Kolkata','Fuel','2026-05-01 09:30:00',0),
('TXN00047','CUST016',3050.00,'Kolkata','Restaurant','2026-05-04 20:30:00',0),
('TXN00048','CUST016',870.00,'Delhi','Grocery','2026-05-06 11:15:00',0),
('TXN00049','CUST017',2400.00,'Hyderabad','Travel','2026-05-02 14:00:00',0),
('TXN00050','CUST017',73000.00,'Mumbai','ATM Withdrawal','2026-05-05 04:00:00',1),
('TXN00051','CUST017',1100.00,'Hyderabad','Grocery','2026-05-08 09:15:00',0),
('TXN00052','CUST018',1900.00,'Pune','Electronics','2026-05-01 15:30:00',0),
('TXN00053','CUST018',2700.00,'Pune','Restaurant','2026-05-03 19:00:00',0),
('TXN00054','CUST018',830.00,'Bangalore','Grocery','2026-05-06 10:45:00',0),
('TXN00055','CUST019',3400.00,'Chennai','Travel','2026-05-02 13:15:00',0),
('TXN00056','CUST019',42000.00,'Delhi','Online Shopping','2026-05-06 01:30:00',1),
('TXN00057','CUST019',1250.00,'Chennai','Fuel','2026-05-08 08:30:00',0),
('TXN00058','CUST020',2050.00,'Kolkata','Restaurant','2026-05-01 20:45:00',0),
('TXN00059','CUST020',3600.00,'Kolkata','Electronics','2026-05-04 12:00:00',0),
('TXN00060','CUST020',950.00,'Mumbai','Grocery','2026-05-07 09:00:00',0);

-- Verify data loaded correctly
SELECT COUNT(*) AS Total_Rows FROM transactions;

-- ============================================
-- STEP 3: FRAUD ANALYSIS QUERIES
-- ============================================

-- Query 1: Overall fraud summary
SELECT 
    COUNT(*) AS Total_Transactions,
    SUM(is_fraud) AS Fraud_Transactions,
    ROUND(SUM(is_fraud) * 100.0 / COUNT(*), 2) AS Fraud_Rate_Percent
FROM transactions;

-- Query 2: High-value transactions (potential fraud threshold)
SELECT transaction_id, customer_id, amount, location, merchant_category, transaction_time
FROM transactions
WHERE amount > 15000
ORDER BY amount DESC;

-- Query 3: Fraud rate by merchant category
SELECT 
    merchant_category,
    COUNT(*) AS Total_Transactions,
    SUM(is_fraud) AS Fraud_Count,
    ROUND(SUM(is_fraud) * 100.0 / COUNT(*), 2) AS Fraud_Rate_Percent
FROM transactions
GROUP BY merchant_category
ORDER BY Fraud_Rate_Percent DESC;

-- Query 4: Fraud rate by location
SELECT 
    location,
    COUNT(*) AS Total_Transactions,
    SUM(is_fraud) AS Fraud_Count,
    ROUND(SUM(is_fraud) * 100.0 / COUNT(*), 2) AS Fraud_Rate_Percent
FROM transactions
GROUP BY location
ORDER BY Fraud_Rate_Percent DESC;

-- Query 5: Customers flagged for fraud, with total fraud amount
SELECT 
    customer_id,
    COUNT(*) AS Fraud_Transactions,
    SUM(amount) AS Total_Fraud_Amount
FROM transactions
WHERE is_fraud = 1
GROUP BY customer_id
ORDER BY Total_Fraud_Amount DESC;

-- Query 6: Fraud vs Legitimate - average amount comparison
SELECT 
    CASE WHEN is_fraud = 1 THEN 'Fraud' ELSE 'Legitimate' END AS Transaction_Type,
    COUNT(*) AS Count,
    ROUND(AVG(amount), 2) AS Avg_Amount,
    ROUND(MIN(amount), 2) AS Min_Amount,
    ROUND(MAX(amount), 2) AS Max_Amount
FROM transactions
GROUP BY is_fraud;

-- Query 7: Anomaly detection - transactions above 3x overall average (subquery)
SELECT transaction_id, customer_id, amount, merchant_category, transaction_time
FROM transactions
WHERE amount > (SELECT AVG(amount) * 3 FROM transactions)
ORDER BY amount DESC;

-- Query 8: Highest transaction per customer (Window Function - RANK)
SELECT customer_id, transaction_id, amount, is_fraud, ranking
FROM (
    SELECT 
        customer_id, 
        transaction_id, 
        amount, 
        is_fraud,
        RANK() OVER (PARTITION BY customer_id ORDER BY amount DESC) AS ranking
    FROM transactions
) AS ranked
WHERE ranking = 1
ORDER BY amount DESC;

-- Query 9: Night-time transactions (12 AM - 5 AM) - common fraud window
SELECT transaction_id, customer_id, amount, transaction_time,
       HOUR(transaction_time) AS Hour_of_Day
FROM transactions
WHERE HOUR(transaction_time) BETWEEN 0 AND 5
ORDER BY transaction_time;

-- Query 10: Daily fraud trend
SELECT 
    DATE(transaction_time) AS Transaction_Date,
    COUNT(*) AS Total_Transactions,
    SUM(is_fraud) AS Fraud_Count
FROM transactions
GROUP BY DATE(transaction_time)
HAVING Fraud_Count > 0
ORDER BY Transaction_Date;
SQL-based fraud detection analysis on transaction data using MySQL - includes window functions, subqueries, and anomaly detection
