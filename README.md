# 🚦 Trafik İşareti Sınıflandırma (Traffic Sign Classification) Projesi

Bu proje, görüntü işleme ve Derin Öğrenme (Deep Learning) teknikleri kullanılarak Almanya Trafik İşaretleri Veri Setindeki farklı trafik işaretini doğru bir şekilde sınıflandırmayı amaçlar.

---

## 🎯 Projenin Amacı

Projenin temel amacı, bir Evrişimli Sinir Ağı (CNN) mimarisi geliştirerek, 32x32 piksel çözünürlüğündeki trafik işareti görüntülerini yüksek doğrulukla ve güvenilirlikle tanımaktır.

Özel hedefler:

1.  Sınıf dengesizliğini (Imbalanced Classes) gidermek.
2.  Küçük boyutlu 32x32 görüntülerden karmaşık özellikler çıkarmak.
3.  Modelin ezberleme (Overfitting) yerine genelleme (Generalization) yapmasını sağlamak.

---

## 📊 Veri Seti Hakkında Bilgi

Bu çalışmada Trafik İşaretleri Veri Seti kullanılmıştır.

| Detay                   | Değer                                                                                                                                                                                                                          |
| :---------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Toplam Sınıf Sayısı** | 43 farklı trafik işareti.                                                                                                                                                                                           |
| **Görüntü Çözünürlüğü** | 32x32 piksel.                                                                                                                                                                                                |
| **Toplam Örnek Sayısı** | 30.000'den fazla görüntü.                                                                                                                                                                                             |
| **Veri Dengesizliği**   | Yüksek düzeyde dengesiz sınıflar mevcuttur. Bazı sınıflar 180 örnek içerirken bazıları 2000 adet veri içerir. |

---

## 🛠 Kullanılan Yöntemler

### 1. Model Mimarisi (TrafficSignCNN)

Önceki VGG mimarilerine dayanan ancak problem ölçeğine indirgenmiş, **4 bloklu derin öğrenme yapısı** kullanılmıştır. Model, küçük boyutlu giriş görüntüsünden 32x32 güçlü özellikler çıkarmak için tasarlanmıştır.

- **Evrişim (Conv) Katmanları:**  32 -> 64 -> 128 filtre sayıları kullanılarak hiyerarşik özellik çıkarımı sağlanmıştır.
- **Normalizasyon:** Batch Normalization (BatchNorm2d) her evrişim bloğunda kullanılarak eğitimin kararlılığı ve hızı artırılmıştır.
- **Sınıflandırıcı:** 2048 girişli, 3 katmanlı tam bağlantılı FC yapı kullanılarak aşırı parametre yükünden kaçınılmıştır.

### 2. Veri Ön İşleme ve Artırım

Modelin genelleme yeteneğini artırmak için **kapsamlı veri artırımı** (Data Augmentation) uygulanmıştır:

- **Normalizasyon:** Veri setinin kendi istatistikleri (Mean ve Std) kullanılarak piksel değerleri normalize edilmiştir.
- **Artırım Teknikleri:** 15° RandomRotation, RandomHorizontalFlip ve ColorJitter kullanılmıştır.

### 3. Optimizasyon ve Düzenleme (Regularization)

- **Kayıp Fonksiyonu:** nn.CrossEntropyLoss kullanılmıştır.
- **Sınıf Ağırlıklandırması (Class Weighting):** Sınıf dengesizliğini gidermek için Loss fonksiyonuna ters frekans ağırlıkları eklenmiştir.
- **Düzenleme:** Dropout=0.4 ve L2 Düzenlemesi (Weight Decay) ile modelin ezberlemesi engellenmiştir.
- **Eğitimi Kontrol:** Early Stopping mekanizması, Validation Loss düşmediğinde eğitimi sonlandırarak en iyi genelleme noktası yakalanmıştır.

---

## 💡 Elde Edilen Sonuçlar

Kullanılan dengeli mimari, veri artırımı ve sınıf ağırlıklandırması stratejileri sayesinde, model Trafik İşaretleri veri setinde beklenen performansı göstermiştir.

| Metrik                                   | Değer                              |
| :--------------------------------------- | :--------------------------------- |
| **Nihai Doğruluk (Validation Accuracy)** | ~83.32                 |
| **Eğitim Süresi**                        | epoch=32 * 2 training      |
| **Model Kapasitesi**                     | Genelleme başarısı orta-yüksektir. |

**Özet:** Model, dengesizlik sorununa rağmen azınlık sınıfları da ortalamanın üstünde bir şekilde öğrenmiş ve 80%'nin üzerinde bir Validation Accuracy hedefine ulaşmıştır.

---

### 🧾 Projenin Linki: https://www.kaggle.com/code/bekirgormez/cnn-classification
