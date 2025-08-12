# BigQuery-Amazon-Sales-Report
Analysis of Amazon Sales Report by BigQuery

Source Data : https://drive.google.com/file/d/1u2LOhVHSFIW5IMxTxhTQ7BX-Bvu1HfzR/view?usp=sharing

--Membersihkan Data

CREATE OR REPLACE TABLE `imad-database.amazon_01.cleaned_sales_report` AS
SELECT
  Date AS order_date,
  `Order ID` AS order_id,
  Status,
  Fulfilment AS fulfilment_type,
  `Sales Channel ` AS sales_channel,
  `ship-service-level` AS shipping_service_level,
  Style,
  SKU,
  Category,
  Size,
  ASIN,
  `Courier Status` AS courier_status,
  Qty,
  currency,
  Amount,
  `ship-city` AS ship_city,
  `ship-state` AS ship_state,
  SAFE_CAST(`ship-postal-code` AS STRING) AS ship_postal_code,
  `ship-country` AS ship_country,
  `promotion-ids` AS promotion_ids,
  B2B,
  `fulfilled-by` AS fulfilled_by
FROM
  `imad-database.amazon_01.sales_report`
WHERE
  Amount IS NOT NULL
  AND currency IS NOT NULL;
  
select * from `amazon_01.cleaned_sales_report`;

