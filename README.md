# YZTA-2026-Datathon-takim-141-
Bilişsel Performans Tahmini - YZTA 2026 Datathon
Bilişsel Performans Tahmini - YZTA 2026 Datathon

Takım: Takim_141
 Bilişsel Performans Tahmini - YZTA 2026 Datathon

Proje Özeti
Bu repo, YZTA-2026 Datathonu için geliştirdiğimiz uçtan uca makine öğrenmesi boru hattını (pipeline) içermektedir. Projenin temel amacı; uyku metrikleri, kafein alımı ve stres seviyeleri gibi biyomedikal ve yaşam tarzı verilerini kullanarak Bilişsel Performans Skorunu en yüksek doğrulukla tahmin etmektir.

Model Gelişim Süreci ve Versiyonlar
Geliştirme sürecimizi ve ulaştığımız en yüksek skorları elde etmek için kullandığımız üç farklı jenerasyonu aşağıda görebilirsiniz:

1. Versiyon 6: K-Fold Çapraz Doğrulama & CatBoost (Temel Model)
Model: CatBoost Regressor

Açıklama: Veri setimiz için sağlam bir temel oluşturduğumuz versiyondur. Aşırı öğrenmeyi (overfitting) engellemek için 3 Farklı Seed ve 5-Fold Cross Validation (Toplam 15 Model) stratejisi kullanılmıştır. Temel özellik mühendisliği (Toplam Kaliteli Uyku, Uyku Bozucu Faktörler ve Tükenmişlik Riski) bu aşamada inşa edilmiştir.

Dosyalar: Takım_141_version_6.ipynb, Takim_141_version_6.csv

2. Versiyon 7: Yarı-Denetimli Öğrenme (CatBoost Pseudo-Labeling)
Model: CatBoost Regressor

Açıklama: Versiyon 6'dan elde edilen yüksek güvenli tahminler test setine entegre edilerek Pseudo-Labeling işlemi yapılmıştır. Eğitim verisi 56.000 satırdan 80.000 satıra çıkarılmış ve K-Fold yerine modelin test dağılımını tamamen ezberleyebilmesi için 5 farklı Seed ile tüm veri üzerinde "Full Fit" (tam eğitim) gerçekleştirilmiştir.

Dosyalar: Takım_141_version_7.ipynb, Takim_141_version_7.csv

3. Versiyon 8: Final Optimizasyon (CatBoost + LightGBM Blending)
Model: LightGBM Regressor & CatBoost Regressor (Blend)

Açıklama: En güçlü performansımızı veren son aşamadır. Versiyon 7'nin skorları hedef değişken (target) olarak alınıp, farklı bir mimari olan LightGBM eğitilmiştir. Kategorik değişkenler LightGBM'in yapısına uygun olarak LabelEncoder ile dönüştürülmüştür. Final tahmini, her iki modelin güçlü yanlarını birleştirmek adına %60 CatBoost ve %40 LightGBM ağırlıklarıyla harmanlanmıştır (Blending).

Dosyalar: takım_141_version_8.ipynb, Takim_141_version_8.csv

🛠️ Teknik Kararlar ve Veri İşleme
Eksik Veri Yönetimi: Nümerik kolonlarda medyan ataması yapılırken (Data Leakage'i önlemek için sadece train üzerinden hesaplanmıştır), kategorik kolonlar "Missing" olarak doldurulup ağaç tabanlı algoritmaların bu durumu kendi başına yorumlamasına izin verilmiştir.

Ağırlıklandırma (Blending): Versiyon 8'de iki farklı mimari birleştirilirken, CatBoost'un bu veri setindeki nispi başarısı göz önüne alınarak ağırlığı (%60) daha yüksek tutulmuştur.

Sınırlandırma (Clipping): Hedef metriğin doğası gereği, final tahminlerinin tamamı np.clip(0, 10) ile 0-10 aralığında tutulmuştur.

📊 Nasıl Çalıştırılır?
train.csv ve test_x.csv dosyalarının ana dizinde olduğundan emin olun.

Versiyonlar birbirine bağlı çalışmaktadır. Versiyon 7'yi çalıştırmak için dizinde Takim_141_version_6.csv dosyası, Versiyon 8'i çalıştırmak için ise Takim_141_version_7.csv dosyası bulunmalıdır.

Gerekli kütüphaneleri yükleyin: pip install catboost lightgbm pandas numpy scikit-learn.

⚖️ Etik ve Akademik Standartlar
Geliştirilen tüm kodlar proje gereksinimlerine uygundur. Rapor içerisindeki akademik atıf formatları, Türkçe metin standartlarına uygun şekilde APA 7 (atıflarda "vd." kullanımı) formatına göre düzenlenmiştir.
