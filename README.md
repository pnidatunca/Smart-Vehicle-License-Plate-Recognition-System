# BTK SERTİFİKA
<img width="1195" height="843" alt="image" src="https://github.com/user-attachments/assets/1005b3aa-d94c-4385-99c0-09a3bb0d10c0" />

# Smart Vehicle License Plate Recognition System
### Proje Hakkında
Bu proje, görüntü işleme teknikleri ve derin öğrenme modellerini birleştirerek çalışan bir güvenlik sistemi simülasyonudur. Projenin temel amacı, kamera görüntüsünden gelen araçları analiz etmek, plakalarını okumak ve belirli kurallara göre (abone mi, yasaklı araç tipi mi) geçiş izni verip vermeyeceğine karar vermektir.Projede hazır kütüphanelerin yanı sıra, hatalı okumaları düzeltmek için kendi geliştirdiğim algoritmaları da kullandım.
### Kullanılan Teknolojiler ve Yöntemler
Proje Python dili kullanılarak Google Colab ortamında geliştirilmiştir. Aşağıdaki temel kütüphane ve modeller kullanılmıştır:

Ultralytics YOLOv8'i görüntüdeki nesneleri tespit etmek için kullandım. Bu model sadece "orada bir araç var" demez, aynı zamanda aracın türünü (Araba, Kamyon, Otobüs) de ayırt eder. Bu sayede kamyonların girmesini yasaklayan bir kural yazabildim.

EasyOCR'ı görüntü üzerindeki metinlerin konumunu (koordinatlarını) bulmak için kullandım.

EasyOCR bazen karakterleri karıştırabiliyordu. Bu yüzden HuggingFace TrOCR(Transformer OCR)'ı da kullandım. Özellikle el yazısına benzer veya bozuk fontlu plakalarda daha iyi sonuç veriyor.

BLIP'i görsel betimleme için kullandım. Aracın rengini veya genel görünümünü metin olarak tarif ediyor (Örneğin: "Mavi bir spor araba").

OpenCV(cv2)'yi görüntüyü işlemek (siyah beyaza çevirmek, gürültüyü temizlemek) ve sonucunda ekrana (o yeşil/kırmızı kutuları) çizdirmek için kullandım.

### Kodun Çalışma Mantığı ve Önemli Kısımlar
Kamera görüntüleri her zaman net olmayabilir. Bu yüzden plakayı okumadan önce görüntüyü netleştiren bir preprocess fonksiyonu ve okunan hatalı harfleri (Örneğin 'O' harfini '0' rakamına çeviren) düzelten bir smart_fix fonksiyonu yazdım.

<img width="905" height="265" alt="image" src="https://github.com/user-attachments/assets/83341172-62fb-42d9-b128-251a26b8fc57" />

<img width="801" height="582" alt="image" src="https://github.com/user-attachments/assets/fe963516-ba58-40de-ba5a-a807d542faa2" />

YOLO modeli ile aracın koordinatlarını bulduktan sonra, sadece o bölgeyi kesip (crop) OCR motoruna gönderdim. Bu işlem sistemin tüm resme bakarak vakit kaybetmesini engelledi.

<img width="613" height="772" alt="image" src="https://github.com/user-attachments/assets/3a88a6eb-9385-453a-805a-3346fae31288" />
<img width="593" height="193" alt="image" src="https://github.com/user-attachments/assets/b9a5254d-c491-4b38-93cc-60288277c940" />

Sistemin en önemli kısmı burasıdır. Okunan plaka ve tespit edilen araç tipi bir mantık süzgecinden geçer. Burada iç içe if-else yapıları kullanarak bir hiyerarşi kurdum.

<img width="836" height="797" alt="image" src="https://github.com/user-attachments/assets/6c3777b3-202a-490c-ba49-a0323acf05c2" />

<img width="712" height="792" alt="image" src="https://github.com/user-attachments/assets/da100e4d-b247-46ab-b5e2-751bbf866855" />

<img width="962" height="710" alt="image" src="https://github.com/user-attachments/assets/4eb30898-601c-42d4-a79d-439e9dbf6957" />



Mantık şu sırayla işler:

Abone Kontrolü: Eğer plaka abone_listesi.json dosyasında varsa, araç tır bile olsa kapı açılır (Yeşil).

Yasaklı Araç Tipi: Eğer abone değilse ve araç "Truck" (Kamyon) veya "Bus" (Otobüs) ise giriş reddedilir.

Kayıtsız Araç: Yukarıdaki şartlara uymuyorsa "Misafir" veya "Kayıtsız" olarak işaretlenir ve giriş reddedilir.

### Test Sonuçları ve Başarı Durumu
Proje kapsamında 9 adet test görseli kullandım çıktıları şu şekildedir:

Araba Çıktııları:

<img width="900" height="1600" alt="image" src="https://github.com/user-attachments/assets/e2484c63-4030-4fb7-9ebd-e78fa4f580f1" />

<img width="1024" height="768" alt="image" src="https://github.com/user-attachments/assets/cd51a7d9-f868-4216-8a99-a910a4097493" />

<img width="792" height="802" alt="image" src="https://github.com/user-attachments/assets/1bd65174-1fbb-4c36-b9d6-8b3d744454b5" />



Otobüs Çıktıları:

<img width="983" height="767" alt="image" src="https://github.com/user-attachments/assets/91622f2f-86cf-410f-9e2f-a2486746f21b" />

<img width="1600" height="1110" alt="image" src="https://github.com/user-attachments/assets/00c6367c-b790-4fec-a8b9-17b671bd432a" />

<img width="1600" height="1066" alt="image" src="https://github.com/user-attachments/assets/7afb0d28-e841-4f10-8cdf-55165853e6e4" />



Tır Çıktıları:

<img width="966" height="666" alt="image" src="https://github.com/user-attachments/assets/5977fe7f-3e10-4d62-b84d-945f68a6ca53" />

<img width="593" height="566" alt="image" src="https://github.com/user-attachments/assets/132f5a6c-27d4-4247-bb42-b4d61fe278a2" />

<img width="733" height="902" alt="image" src="https://github.com/user-attachments/assets/76f85910-4078-4434-9f8c-32f376487f9b" />



YOLOv8 modeli araçları %90'ın üzerinde başarıyla ayırt edebilmektedir.

Sistem, abone listesine yeni bir araç eklendiğinde kodu durdurup başlatmaya gerek kalmadan listeyi güncelleyebiliyor.

Standart plakalarda okuma başarısı orta seviyededir.Özellikle "O" ve "0" veya "B" ve "8" karışıklığı) gibi hatalar gözlemlenmiştir. Bu durum kod içerisinde karakter düzeltme algoritmaları ile minimize edilmeye çalışılsa da bazı çıktı sonuçlarında görebiliyoruz. 

Tırlarda plaka okuma konusun da diğer araçlara kıyasla daha kötü bir performans elde ettik.

### Geliştirmeye Açık Yönler
Bu proje bir prototip olup, ileride şu eklemelerle geliştirilebilir:
1. Plaka okuma kısmında iyileştirmeler yapılabilir.
2. Gerçek zamanlı bir kameraya bağlanarak canlı sistem haline getirilebilir.
3. Veritabanı olarak JSON yerine SQL veya Firebase kullanılarak web tabanlı bir yönetim paneli eklenebilir.
4. Giriş yapan araçların tarih ve saat bilgisiyle loglanması sağlanabilir.

### Kurulum
Bu proje Google Colab üzerinde geliştirilmiştir. Çalıştırmak için .ipynb dosyasını açıp sırasıyla hücreleri çalıştırmanız ve test fotoğraflarını yüklemeniz yeterlidir.
