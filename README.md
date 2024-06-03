# Big-Data-Analytics-Kimia-Farma

CREATE TABLE kimia_farma.Rakamin_KF_Analytics AS
/*Syntax SELECT untuk menampilkan informasi database*/
SELECT  
  ft.transaction_id,
  ft.date,
  ft.branch_id,
  kc.branch_name,
  kc.kota,
  kc.provinsi,
  ft.rating AS rating_cabang,
  ft.customer_name,
  p.product_id,
  p.product_name,
  p.price AS actual_price,
  ft.discount_percentage,
  CASE
    WHEN p.price <= 50000 THEN p.price * 0.10
    WHEN p.price <= 100000 THEN p.price * 0.15
    WHEN p.price <= 300000 THEN p.price * 0.20
    WHEN p.price <= 500000 THEN p.price * 0.25
    ELSE p.price * 0.30
  END AS persentage_gross_laba,
  (p.price * (1 - ft.discount_percentage / 100)) AS nett_sales,
  (p.price * (1 - ft.discount_percentage / 100)) * (CASE
                                                            WHEN p.price <= 50000 THEN 0.10
                                                            WHEN p.price <= 100000 THEN 0.15
                                                            WHEN p.price <= 300000 THEN 0.20
                                                            WHEN p.price <= 500000 THEN 0.25
                                                            ELSE 0.3
                                                        END) AS nett_profit,
  ft.rating AS rating_transaksi
FROM 
  kimia_farma.kf_final_transaction ft
INNER JOIN
  kimia_farma.kf_kantor_cabang kc
on 
  ft.branch_id = kc.branch_id
INNER JOIN 
  kimia_farma.kf_product p
on 
  ft.product_id = p.product_id
    
