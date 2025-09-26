# 🚦 Trafik İşareti Sınıflandırma (Traffic Sign Classification) Projesi

Bu proje, görüntü işleme ve Derin Öğrenme $(\mathbf{Deep } \mathbf{Learning})$ teknikleri kullanılarak **Almanya Trafik İşaretleri Veri Setindeki $(\text{GTSRB})$** $43$ farklı trafik işaretini doğru bir şekilde sınıflandırmayı amaçlar.

---

## 🎯 Projenin Amacı

Projenin temel amacı, bir Evrişimli Sinir Ağı $(\mathbf{Convolutional } \mathbf{Neural } \mathbf{Network} - \mathbf{CNN})$ mimarisi geliştirerek, $\mathbf{32 \times 32}$ piksel çözünürlüğündeki trafik işareti görüntülerini yüksek doğrulukla $(\mathbf{\text{Accuracy}})$ ve güvenilirlikle $(\mathbf{F1 } \mathbf{Score})$ tanımaktır.

Özel hedefler:

1.  Sınıf dengesizliğini $(\mathbf{Imbalanced } \mathbf{Classes})$ gidermek.
2.  Küçük boyutlu $(\mathbf{32 \times 32})$ görüntülerden karmaşık özellikler çıkarmak.
3.  Modelin ezberleme $(\mathbf{Overfitting})$ yerine genelleme $(\mathbf{Generalization})$ yapmasını sağlamak.

---

## 📊 Veri Seti Hakkında Bilgi

Bu çalışmada $\mathbf{\text{Almanya } \text{Trafik } \text{İşaretleri } \text{Veri } \text{Seti } (\text{GTSRB})}$ kullanılmıştır.

| Detay                   | Değer                                                                                                                                                                                                                          |
| :---------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Toplam Sınıf Sayısı** | $\mathbf{43}$ farklı trafik işareti.                                                                                                                                                                                           |
| **Görüntü Çözünürlüğü** | $\mathbf{32 \times 32}$ piksel.                                                                                                                                                                                                |
| **Toplam Örnek Sayısı** | $\mathbf{\approx 30.000}$ görüntü.                                                                                                                                                                                             |
| **Veri Dengesizliği**   | Yüksek düzeyde dengesiz sınıflar mevcuttur. $(\text{Örn: } \text{Bazı } \text{sınıflar } \mathbf{\approx 180 } \text{adet } \text{örnek, } \text{bazıları } \mathbf{\approx 2000 } \text{adet } \text{örnek } \text{içerir.})$ |

---

## 🛠 Kullanılan Yöntemler

### 1. Model Mimarisi (TrafficSignCNN)

Önceki VGG mimarilerine dayanan ancak problem ölçeğine indirgenmiş, $\mathbf{4 \text{ } \text{bloklu } \text{derin } \text{bir } \text{CNN } \text{yapısı }}$ kullanılmıştır. Model, küçük boyutlu giriş görüntüsünden $(\mathbf{32 \times 32 \times 3})$ güçlü özellikler çıkarmak için tasarlanmıştır.

- **Evrişim (Conv) Katmanları:** $\mathbf{32} \rightarrow \mathbf{64} \rightarrow \mathbf{128} $ filtre sayıları kullanılarak hiyerarşik özellik çıkarımı sağlanmıştır.
- **Normalizasyon:** $\mathbf{\text{Batch } \text{Normalization } (\text{BatchNorm2d})}$ her evrişim bloğunda kullanılarak eğitimin kararlılığı ve hızı artırılmıştır.
- **Sınıflandırıcı:** $\mathbf{2048}$ girişli, $\mathbf{3}$ katmanlı tam bağlantılı $(\mathbf{FC})$ yapı kullanılarak aşırı parametre yükünden kaçınılmıştır.

### 2. Veri Ön İşleme ve Artırım

Modelin genelleme yeteneğini artırmak için **kapsamlı veri artırımı** $(\mathbf{Data } \mathbf{Augmentation})$ uygulanmıştır:

- **Normalizasyon:** Veri setinin kendi istatistikleri $(\mathbf{\text{Mean } \text{ve } \text{Std}})$ kullanılarak piksel değerleri normalize edilmiştir.
- **Artırım Teknikleri:** $\mathbf{\text{RandomRotation } (\mathbf{15 } \text{derece})}, \mathbf{\text{RandomHorizontalFlip}}$ ve $\mathbf{\text{ColorJitter}}$ kullanılmıştır.

### 3. Optimizasyon ve Düzenleme (Regularization)

- **Kayıp Fonksiyonu:** $\mathbf{\text{nn.CrossEntropyLoss}}$ kullanılmıştır.
- **Sınıf Ağırlıklandırması (Class Weighting):** Sınıf dengesizliğini gidermek için $\mathbf{\text{Loss } \text{fonksiyonuna } \text{ters } \text{frekans } \text{ağırlıkları }}$ eklenmiştir.
- **Düzenleme:** $\mathbf{\text{Dropout } (\mathbf{p}=0.4)}$ ve $\mathbf{\text{L2 } \text{Düzenlemesi } (\mathbf{Weight } \mathbf{Decay})}$ ile modelin ezberlemesi engellenmiştir.
- **Eğitimi Kontrol:** $\mathbf{\text{Early } \text{Stopping}}$ mekanizması, $\mathbf{\text{Validation } \text{Loss}}$ düşmediğinde eğitimi sonlandırarak en iyi genelleme noktası yakalanmıştır.

---

## 💡 Elde Edilen Sonuçlar

Kullanılan dengeli mimari, veri artırımı ve sınıf ağırlıklandırması stratejileri sayesinde, model $\mathbf{\text{GTSRB } \text{veri } \text{setinde } \text{beklenen } \text{performansı } \text{göstermiştir.}}$

| Metrik                                   | Değer                              |
| :--------------------------------------- | :--------------------------------- |
| **Nihai Doğruluk (Validation Accuracy)** | $\mathbf{83.32\%}$                 |
| **Eğitim Süresi**                        | $\approx \mathbf{32*2}$ epoch      |
| **Model Kapasitesi**                     | Genelleme başarısı orta-yüksektir. |

**Özet:** Model, dengesizlik sorununa rağmen azınlık sınıfları da ortalamanın üstünde bir şekilde öğrenmiş ve $\mathbf{80\%}$'ın üzerinde bir $\mathbf{\text{Validation } \text{Accuracy}}$ hedefine ulaşmıştır.

---

### 🧾 Projenin Linki: https://www.kaggle.com/code/bekirgormez/cnn-classification
