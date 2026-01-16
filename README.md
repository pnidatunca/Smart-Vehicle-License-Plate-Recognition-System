# Smart Vehicle License Plate Recognition System
### Proje Hakkında
Bu proje, kampüs, site veya özel otopark girişlerinde kullanılabilecek otomatik bir güvenlik sistemi prototipidir. Sistemin temel amacı, güvenlik kameralarından alınan görüntülerdeki araçları analiz ederek; araç tipine (otomobil, tır, otobüs) ve plaka yetkisine göre kapının açılıp açılmayacağına karar vermektir.
Bu projede sadece plaka okuma değil, aynı zamanda aracın görsel analizi (tip tespiti ve betimlemesi) de yapılarak çok katmanlı bir güvenlik mekanizması oluşturulmuştur.

### Kullanılan Teknolojiler ve Yöntemler
Proje Python dili kullanılarak Google Colab ortamında geliştirilmiştir. Aşağıdaki temel kütüphane ve modeller kullanılmıştır:
* **YOLOv8 (Ultralytics):** Görüntüdeki araçların tespiti ve sınıflandırılması (Araba, Tır, Otobüs vb.) için kullanıldı.
* **EasyOCR:** Plakanın görüntü üzerindeki konumunu tespit etmek için kullanıldı.
* **TrOCR (Transformer OCR):** Tespit edilen plaka bölgesindeki karakterlerin optik karakter tanıma (OCR) işlemi için Microsoft'un transformer tabanlı modeli tercih edildi.
* **BLIP (Bootstrapping Language-Image Pre-training):** Aracın rengi ve görsel özellikleri hakkında yapay zeka tabanlı yorum almak için kullanıldı.
* **OpenCV:** Görüntü işleme, filtreleme (CLAHE) ve çizim işlemleri için kullanıldı.

### Sistemin Çalışma Mantığı
Sistem sırasıyla şu adımları izler:
1. **Görüntü Alma:** Test klasöründen alınan görüntü işlenir.
2. **Nesne Tespiti:** YOLO modeli ile araçlar bulunur ve koordinatları çıkarılır.
3. **Plaka Konumlandırma ve Okuma:** Aracın ön/arka bölgesine odaklanılarak plaka bölgesi kesilir. EasyOCR ve TrOCR hibrit bir yapıda çalıştırılarak plaka metni elde edilir. Hatalı okumaları önlemek için Regex (Düzenli İfadeler) ile format kontrolü yapılır.
4. **Karar Mekanizması:**
    * Eğer okunan plaka "abone listesinde" (JSON veritabanı) kayıtlıysa, araç tipine bakılmaksızın geçiş izni verilir.
    * Kayıtlı değilse ve araç tipi Tır veya Otobüs ise "Yasaklı Araç Tipi" gerekçesiyle reddedilir.
    * Kayıtlı olmayan otomobiller "Misafir/Yetkisiz" olarak işaretlenir.

### Test Sonuçları ve Başarı Durumu
Proje kapsamında 12 adet test görseli kullanılmıştır.
* **Araç Tipi Tespiti:** YOLOv8 modeli araçları %90'ın üzerinde başarıyla ayırt edebilmektedir.
* **Plaka Okuma:** Standart plakalarda okuma başarısı orta seviyededir.Özellikle "O" ve "0" veya "B" ve "8" karışıklığı) gibi hatalar gözlemlenmiştir. Bu durum kod içerisinde karakter düzeltme algoritmaları ile minimize edilmiştir.
* **Kısıtlar:** Tır plakaları gibi standart dışı konumlarda (tampon altı vb.) veya çok silik yazılarda sistemin performansı düşebilmektedir.

### Geliştirmeye Açık Yönler
Bu proje bir prototip olup, ileride şu eklemelerle geliştirilebilir:
1. Plaka okuma kısmında iyileştirmeler yapılabilir.
2. Gerçek zamanlı bir kameraya bağlanarak canlı sistem haline getirilebilir.
3. Gece görüşü için kızılötesi filtreleme algoritmaları eklenebilir.
4. Veritabanı olarak JSON yerine SQL veya Firebase kullanılarak web tabanlı bir yönetim paneli eklenebilir.
5. Giriş yapan araçların tarih ve saat bilgisiyle loglanması sağlanabilir.

### Kurulum
Kodları çalıştırmak için `Final_Proje.ipynb` dosyasını Google Colab ile açmanız ve gerekli kütüphaneleri (ilk hücrede belirtilen) yüklemeniz yeterlidir. Veritabanı dosyası (`abone_listesi.json`) kod çalıştırıldığında otomatik olarak oluşturulmaktadır.
