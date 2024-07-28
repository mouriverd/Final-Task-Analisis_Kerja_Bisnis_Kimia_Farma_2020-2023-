# Final-Task-Analisis_Kerja_Bisnis_Kimia_Farma_2020-2023-
This is my final task project as a Big Analytics intern at Kimia Farma for processing and analysis data in evaluate business project 2020-2023

---Kimia Farma _Big Data Anaytics Challenge
SELECT
    t.transaction_id, -- Kode id transaksi dari tabel kf_final_transaction
    t.date, -- Tanggal transaksi dilakukan dari tabel kf_final_transaction
    t.branch_id, -- Kode id cabang dari tabel kf_final_transaction
    b.branch_name, -- Nama cabang kimia farma dari tabel kf_kantor_cabang
    b.kota, -- Kota cabang kimia farma dari tabel kf_kantor_cabang
    b.provinsi, -- Provinsi cabang kimia farma dari tabel kf_kantor_cabang
    b.rating AS rating_cabang, -- penilaian konsumen terhadap cabang kimia farma dari tabel kf_kantor_cabang
    t.customer_name, -- Nama customer yang melakukan transaksi dari tabel kf_final_transaction
    t.product_id, -- Kode produk obat dari tabel kf_final_transaction
    p.product_name, -- Nama obat dari tabel kf_product
    t.price AS actual_price, -- Harga obat dari tabel kf_final_transaction
    t.discount_percentage, -- Persentase diskon pada obat dari tabel kf_final_transaction
    CASE
        WHEN t.price <= 50000 THEN 0.10
        WHEN t.price > 50000 AND t.price <= 100000 THEN 0.15
        WHEN t.price > 100000 AND t.price <= 300000 THEN 0.20
        WHEN t.price > 300000 AND t.price <= 500000 THEN 0.25
        ELSE 0.30
    END AS persentase_gross_laba, -- Persentase laba kotor diterima  dari obat
    t.price * (1 - t.discount_percentage) AS nett_sales, -- harga setelah diskon
    (t.price * (1 - t.discount_percentage)) * CASE
        WHEN t.price <= 50000 THEN 0.10
        WHEN t.price > 50000 AND t.price <= 100000 THEN 0.15
        WHEN t.price > 100000 AND t.price <= 300000 THEN 0.20
        WHEN t.price > 300000 AND t.price <= 500000 THEN 0.25
        ELSE 0.30
    END AS nett_profit, -- keuntungan bersih berdasarkan penjualan bersih dan persentase laba
    t.rating AS rating_transaksi -- Penilaian konsumen terhadap transaksi dari tabel kf_final_transaction
FROM
    kimia_farma.kf_final_transaction t -- Tabel kf_final_transaction dengan alias t
JOIN
    kimia_farma.kf_kantor_cabang b ON t.branch_id = b.branch_id -- Join dengan tabel kf_kantor_cabang berdasarkan branch_id
JOIN
    kimia_farma.kf_product p ON t.product_id = p.product_id; -- Join dengan tabel kf_product berdasarkan product_id
