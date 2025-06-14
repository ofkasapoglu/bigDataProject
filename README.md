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
- **Kaynak:** [Kaggle/Malicious URLs dataset](https://www.kaggle.com/datasets/sid321axn/malicious-urls-dataset/code)
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
```
🌲 Random Forest
200 ağaç, max_depth=20

class_weight="balanced" ile sınıf dengesizliği önlenmiştir.

Test seti ile karşılaştırmalı analiz yapılmıştır.

🖥️ Gradio Arayüzü
```
gr.Interface(
    fn=predict_url_with_proba,
    inputs=gr.Textbox(label="URL'yi girin"),
    outputs=gr.Textbox(label="Tahmin Sonucu"),
    title="URL Güvenlik Tahmin Aracı",
    description="Bu araç, bir URL'nin güvenli olup olmadığını analiz eder.",
    theme="default"
).launch(share=True)
```

🚀 Projeyi Çalıştırma
1. Ortam Hazırlığı
```
pip install pandas numpy matplotlib seaborn scikit-learn xgboost gradio joblib
```
2. Çalıştır
```
python bigDataProject.py
```
Gradio arayüzü otomatik olarak açılacaktır.

🔐 Güvenlik Notu
Bu proje, eğitim ve araştırma amaçlıdır. Gerçek zamanlı güvenlik sistemlerinde bu tür modellerin gelişmiş tehdit analiz sistemleriyle birlikte kullanılması gerekir. Bu sistem, sadece URL metnine dayanarak tahmin yapar.

📄 Lisans
MIT Lisansı altında yayınlanmıştır. Ticarî kullanım için geliştirici izni gereklidir.

✍️ Geliştirici
📛 [Ömer Faruk Kasapoğlu]
📧 [kasapoglu2100@gmail.com]
💼 [linkedin.com/ÖmerFarukKasapoğlu](https://www.linkedin.com/in/ömer-faruk-kasapoğlu-26a7b0262/)
