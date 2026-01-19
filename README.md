# Smart Vehicle License Plate Recognition System
### Proje Hakkında
Bu proje, görüntü işleme teknikleri ve derin öğrenme modellerini birleştirerek çalışan bir güvenlik sistemi simülasyonudur. Projenin temel amacı, kamera görüntüsünden gelen araçları analiz etmek, plakalarını okumak ve belirli kurallara göre (abone mi, yasaklı araç tipi mi) geçiş izni verip vermeyeceğine karar vermektir.Projede hazır kütüphanelerin yanı sıra, hatalı okumaları düzeltmek için kendi geliştirdiğim algoritmaları da kullandım.
### Kullanılan Teknolojiler ve Yöntemler
Proje Python dili kullanılarak Google Colab ortamında geliştirilmiştir. Aşağıdaki temel kütüphane ve modeller kullanılmıştır:

Ultralytics YOLOv8'i Görüntüdeki nesneleri tespit etmek için kullandım. Bu model sadece "orada bir araç var" demez, aynı zamanda aracın türünü (Araba, Kamyon, Otobüs) de ayırt eder. Bu sayede kamyonların girmesini yasaklayan bir kural yazabildim.

EasyOCR'ı Görüntü üzerindeki metinlerin konumunu (koordinatlarını) bulmak için kullandım.

HuggingFace TrOCR(Transformer OCR)'ı EasyOCR bazen karakterleri karıştırabiliyordu. Bu yüzden Microsoft'un TrOCR modelini entegre ettim. Özellikle el yazısına benzer veya bozuk fontlu plakalarda daha iyi sonuç veriyor.

BLIP'i Projeye ekstra bir özellik olarak ekledim. Aracın rengini veya genel görünümünü metin olarak tarif ediyor (Örneğin: "Mavi bir spor araba").

OpenCV(cv2)'yi Görüntüyü işlemek (siyah beyaza çevirmek, gürültüyü temizlemek) ve sonucunda ekrana o yeşil/kırmızı kutuları çizdirmek için kullandım.

### Kodun Çalışma Mantığı ve Önemli Kısımlar
Kamera görüntüleri her zaman net olmayabilir. Bu yüzden plakayı okumadan önce görüntüyü netleştiren bir preprocess fonksiyonu ve okunan hatalı harfleri (Örneğin 'O' harfini '0' rakamına çeviren) düzelten bir smart_fix fonksiyonu yazdım.
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
