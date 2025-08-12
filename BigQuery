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


--10 Produk Terlaris berdasarkan SKU

SELECT
  SKU,         -- Kode unik produk
  Category,    -- Kategori produk
  COUNT(DISTINCT order_id) AS total_orders,  -- Jumlah pesanan unik
  SUM(Qty) AS total_quantity,                -- Jumlah unit terjual
  SUM(Amount) AS total_sales                 -- Total nilai penjualan
FROM
  `imad-database.amazon_01.cleaned_sales_report`
WHERE
  LOWER(Status) LIKE 'shipped%'  -- Filter hanya produk yang sudah dikirim
GROUP BY
  SKU, Category
ORDER BY
  total_sales DESC  -- Ambil yang paling tinggi nilainya
LIMIT 10;

--Penjualan Berdasarkan Kota (Top 10)

SELECT
  ship_city,  -- Nama kota pengiriman
  COUNT(DISTINCT order_id) AS total_orders,  -- Jumlah pesanan ke kota tersebut
  SUM(Qty) AS total_quantity,                -- Total produk terkirim
  SUM(Amount) AS total_sales                 -- Total penjualan ke kota
FROM
  `imad-database.amazon_01.cleaned_sales_report`
WHERE
  LOWER(Status) LIKE 'shipped%'  -- Hanya pesanan yang berhasil dikirim
GROUP BY
  ship_city
ORDER BY
  total_sales DESC  -- Ranking kota berdasarkan nilai penjualan
LIMIT 10;

--Ringkasan Penjualan Bulanan

SELECT
  FORMAT_DATE('%Y-%m', order_date) AS month, -- Format bulan-tahun, contoh: 2025-08
  COUNT(DISTINCT order_id) AS total_orders,  -- Total pesanan dalam bulan tersebut
  SUM(Qty) AS total_quantity,                -- Jumlah unit produk yang terjual
  SUM(Amount) AS total_sales                 -- Total pendapatan dari penjualan
FROM
  `imad-database.amazon_01.cleaned_sales_report`
WHERE
  LOWER(Status) LIKE 'shipped%'  -- Hanya pesanan yang terkirim
GROUP BY
  month
ORDER BY
  month;  -- Urutkan berdasarkan urutan bulan

-- Menghitung total penjualan berdasarkan kategori
-- Hanya mencakup pesanan dengan status "shipped" (dikirim)

SELECT
  Category,  -- Nama kategori produk
  COUNT(DISTINCT order_id) AS total_orders,  -- Total pesanan unik per kategori
  SUM(Qty) AS total_quantity,                -- Total kuantitas barang terjual
  SUM(Amount) AS total_sales                 -- Total nilai penjualan
FROM
  `imad-database.amazon_01.cleaned_sales_report`
WHERE
  LOWER(Status) LIKE 'shipped%'  -- Hanya ambil status pesanan yang terkirim
GROUP BY
  Category
ORDER BY
  total_sales DESC;  -- Urutkan berdasarkan penjualan tertinggi

-- Menampilkan tren penjualan harian sepanjang waktu

SELECT
  order_date,  -- Tanggal pesanan dilakukan
  COUNT(DISTINCT order_id) AS total_orders,  -- Jumlah pesanan per hari
  SUM(Qty) AS total_quantity,                -- Jumlah barang terjual per hari
  SUM(Amount) AS total_sales                 -- Total nilai penjualan per hari
FROM
  `imad-database.amazon_01.cleaned_sales_report`
WHERE
  LOWER(Status) LIKE 'shipped%'  -- Hanya pesanan yang terkirim
GROUP BY
  order_date
ORDER BY
  order_date;  -- Urutkan kronologis berdasarkan tanggal
