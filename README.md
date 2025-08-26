# İTÜ Cezeri Takımı – Trendyol E-Ticaret Hackathon 2025

Bu repo, İTÜ Cezeri Takımı olarak katıldığımız **Trendyol E-Ticaret Hackathon 2025** yarışması kapsamında geliştirdiğimiz çözüm pipeline’ını içermektedir. Çözüm, uçtan uca veri hazırlama, model eğitimi ve tahminleme adımlarını kapsamaktadır.

---

## Pipeline Aşamaları

### 1. Veri Hazırlama (Data Preparation)
- **Kullanılan kütüphaneler:** `polars`, `gc`
- Ham veriler (`train`, `test` vb.) `DATA_PATH` altından okunmuştur.
- `polars` kütüphanesi sayesinde hızlı ve bellek dostu veri işleme yapılmıştır.
- Tablo birleştirme (join), özellik seçimi ve eksik değer işlemleri gerçekleştirilmiştir.
- **Kategori düzenlemeleri:** 
  - Bazı kategoriler önemli olduğu **direkt eklenmiştir**.
      ordered
      clicked
      added_to_cart
      added_to_fav
      attribute_type_count
      total_attribute_option_count
      merchant_count
      filterable_label_count

- Veri dağılımında bazı kategoriler **birleştirilmiş ve yeniden oluşturulmuştur**.
    "content_price_data_avg_content_review_count", "content_price_data_avg_content_review_wth_media_count","content_price_data_avg_content_rate_count", "content_price_data_avg_content_rate_avg", "max_price",          "min_price", "discount_rate", "total_rate_score","media_review_ratio", "attribute_type_count", "total_attribute_option_count", "merchant_count", "filterable_label_count","user_age", "user_tenure_in_years",
    "user_gender_encoded", "user_sitewide_avg_total_click", "user_sitewide_avg_total_cart", "user_sitewide_avg_total_fav", "user_sitewide_avg_total_order","user_sitewide_avg_click_cart_order_ratio",                   "user_sitewide_avg_click_cart_ratio", "user_sitewide_avg_click_order_ratio", "user_fashion_sitewide_avg_total_click", "user_fashion_sitewide_avg_total_cart","user_fashion_sitewide_avg_total_fav", 
    "user_fashion_sitewide_avg_total_order", "user_fashion_sitewide_avg_click_cart_order_ratio", "user_fashion_sitewide_avg_click_cart_ratio", "user_fashion_sitewide_avg_click_order_ratio",                            "content_sitewide_avg_total_click", "content_sitewide_avg_total_cart","content_sitewide_avg_total_fav", "content_sitewide_avg_total_order", "content_sitewide_avg_click_cart_order_ratio", 
    "content_sitewide_avg_click_cart_ratio", "content_sitewide_avg_click_order_ratio","content_top_terms_log_avg_total_search_click", "content_top_terms_log_avg_total_search_impression",                               "content_top_terms_log_avg_click_impression_ratio", "user_top_terms_log_avg_total_search_click", "user_top_terms_log_avg_total_search_impression", "user_top_terms_log_avg_click_impression_ratio",
    "user_fashion_search_log_avg_total_search_click", "user_fashion_search_log_avg_total_search_impression", "user_fashion_search_log_avg_click_impression_ratio","term_search_log_avg_total_search_click",              "term_search_log_avg_total_search_impression", "term_search_log_avg_click_impression_ratio"
    ]


---

### 2. Model Eğitimi (Model Training)
- **Kullanılan kütüphane:** `lightgbm`
- Hazırlanan özelliklerle sınıflandırma/regresyon modeli eğitilmiştir.
- `LightGBM` seçilme nedeni:
  - Büyük veri setlerinde yüksek hız ve verimlilik,
  - Otomatik özellik seçimi.
- Parametre ayarları (`num_leaves`, `learning_rate`, `n_estimators`, 'feature_fraction', 'bagging_fraction', 'max_depth', 'lambda_l1', 'lambda_l2', 'min_split_gain', 'min_child_weight') optimize edilmiştir.
- Ordered, clicked, added_to_cart, added_to_fav için birer kez olmak koşuluyla dört model eğitilmiş ve belirtilen ağırlıklar(weights) ile modelin tahmin çıktısı oluşturulmuştur.


---

### 3. Tahminleme ve Çıktı Üretme (Prediction & Submission)
- Eğitilen model, test seti üzerinde tahmin üretmiştir.
- Test seti ekleme order-clicked ve added_to_cart-added_to_fav modelleri için ayrı ayrı yapılarak modelin tahmin sonucu arttırılmıştır.
- `model.predict()` ile skorlar elde edilmiştir.
- Sonuçlar istenilen formatta (`user_id`, `item_id`, `prediction`) düzenlenmiştir.
- Nihai dosya `submission.csv` olarak kaydedilmiştir.

---

## Ortam (Environment)
Kullanılan başlıca bağımlılıklar:
- `polars`
- `lightgbm`
- `pyarrow`
- `gc` (Python standart kütüphanesi)

Bağımlılıklar `requirements.txt` ve `environment.yml` dosyalarıyla paylaşılmıştır.

---

## Çalıştırma Talimatı

1. Ortam kurulumu:
   ```bash
   conda env create -f environment.yml && conda activate trendyol-hackathon
   # veya
   pip install -r requirements.txt

