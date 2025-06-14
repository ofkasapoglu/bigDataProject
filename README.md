# 🛡️ Kötü Amaçlı URL Tespiti (Malicious URL Detection with Machine Learning)

Bu proje, kullanıcıların ziyaret etmeyi planladığı web sitelerinin güvenliğini analiz etmeye yardımcı olan bir **zararlı URL tespit sistemi**dir. Proje, veri madenciliği ve makine öğrenmesi tekniklerini kullanarak bir URL'nin **zararlı (malicious)** mı yoksa **güvenli (benign)** mi olduğunu tahmin eder.

## 🎯 Projenin Amacı

Siber saldırıların önemli bir kısmı kullanıcıların kötü amaçlı URL'lere yönlendirilmesiyle gerçekleşmektedir. Bu proje ile amaçlanan, herhangi bir URL’nin metinsel özelliklerine ve karakteristik yapılarına bakarak, olası tehlikeleri **önceden** tahmin edebilen akıllı bir sistem geliştirmektir.

## 🔍 Uygulama Özeti

- **TF-IDF** ve özel olarak çıkarılan istatistiksel özellikler kullanılarak URL’ler sayısal verilere dönüştürülmüştür.
- **XGBoost** ve **Random Forest** algoritmalarıyla eğitim yapılmış, model başarımları karşılaştırılmıştır.
- **Gradio** ile kullanıcı dostu bir web arayüzü tasarlanarak modelin kullanımı kolaylaştırılmıştır.

---

## 📚 Kullanılan Veri Seti

- **Dosya Adı:** `malicious_dataset_with_10000_benign.csv`
- **Kaynak:** [Kaggle/Benzer kamu veri setleri](https://www.kaggle.com/)
- **Veri Yapısı:**  
  - `url`: İncelenecek web adresi  
  - `type`: Etiket (benign / phishing / defacement / malware)

Toplamda **10.000+ benign** ve çok sayıda malicious URL örneği barındıran dengeli bir veri setidir.

---

## 🛠️ Kullanılan Kütüphaneler

- `pandas`, `numpy`, `matplotlib`, `seaborn`
- `scikit-learn`: Veri işleme, değerlendirme, modelleme
- `xgboost`: XGBoost modeli
- `gradio`: Arayüz oluşturma
- `joblib`: Model ve nesne kaydetme/yükleme

---

## 🧪 Özellik Mühendisliği

### 🔡 TF-IDF Özellikleri
- URL’lerdeki kelime ve karakter örüntüleri `TfidfVectorizer` ile sayısal vektörlere dönüştürüldü.
- 2 karakterden uzun harf/rakam örüntüleri filtrelendi.
- `max_features=10,000`, `min_df=5`, `max_df=0.8` gibi parametrelerle nadir ve çok sık geçen örüntüler ayıklandı.

### 📊 Ekstra Özellikler
Ayrıca, URL’lerin istatistiksel ve yapısal özellikleri de çıkarılmıştır:
- `url_length`: URL'nin uzunluğu  
- `num_digits`: Sayı karakterlerinin sayısı  
- `num_special_chars`: Noktalama işaretleri ve özel karakter sayısı  
- `has_https`: HTTPS içerip içermemesi (0/1)  
- `num_subdomains`: Nokta sayısı ile subdomain tahmini

Bu iki özellik kümesi birleştirilerek model giriş verisi olarak kullanılmıştır.

---

## 🤖 Kullanılan Modeller ve Parametreler

### ✅ XGBoost

```python
model = XGBClassifier(
    tree_method="gpu_hist",
    use_label_encoder=False,
    eval_metric="logloss",
    n_estimators=300,
    max_depth=8,
    learning_rate=0.05,
    subsample=0.8,
    colsample_bytree=0.8,
    scale_pos_weight=(benign/malicious dengesi),
    random_state=42
)
