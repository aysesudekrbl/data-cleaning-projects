# NYC Airbnb 2019 Data Cleaning

- İçerik: New York City Airbnb Open Data 2019 (Kaggle) data seti üzerinde uçtan uca bir data cleaning çalışması.

- Amaç: Gerçek dünya verisinde karşılaşılabilecek eksik değer, mantık hatası ve tutarsızlıkları tespit edip, veri kaybı yaşanmadan düzeltilmesi.

## Dataset

- Kaynak: [New York City Airbnb Open Data](https://www.kaggle.com/datasets/dgomonov/new-york-city-airbnb-open-data)

- Toplam: 48,895 satır, 16 sütun


## Bulunan Sorunlar ve Çözümler

### 1. Eksik review verisi
- “reviews_per_month” sütununda 10,052 eksik değer vardı.
- Bu satırların tamamında “number_of_reviews” değeri 0 çıktı. Bu bize hiç yorum bulunmadığını, yani aylık yorum sayısının hesaplanamadığını gösterdi. 
- “reviews_per_month” sütunundaki eksik değerler 0 ile dolduruldu.
- “last_review” sütunundaki eksikler eğer “number_of_reviews”, yani bulunan yorum sayısı 0 ile “Doesn’t exist”, değil ise “Couldn’t find” (bu veri setinde bulunmadı) olarak dolduruldu. Dolu olan tarihlere dokunulmadı.

### 2. Eksik “name” ve “host_name”
- Sırasıyla 16 ve 21 satırda bu veriler bulunmuyordu. Sayı çok az ve tahmin edilebilmesini sağlayacak herhangi bir bilgi bulunmadığından “Unknown” ile dolduruldu.

### 3. Mantıksal Çelişki (“minimum_nights” ve “availability_365” arasında)
- “minimum_nights” yani kiralanırken kalınması gereken minimum gün sayısı sütununun değerlerinde max değer 1250 gece çıktı. (Medyan 3, Ortalama 7) Bu bize ciddi bir aykırılık olduğunu gösteriyor.
- “minimum_nights” > “availability_365” olan 14 satır bulundu. Bir ilan müsait olduğu gün sayısından daha fazla gün boyunca kiralanamaz, dolayısıyla daha büyük bir minimum gün isteme imkanı yoktur. Bu durum mantıksal olarak bir imkansızlık sunuyor.
- Bu 14 satır **silinmedi**, orijinal veri korunarak iki yeni sütunla işaretlendi:
       “is_unrealistic_min_nights”: Bu mantıksızlığı taşıyan satırları ‘True’ olarak flagler.
       “minimum_nights_adjusted”: “minimum_nights” ve “availability_365” değerlerinden minimum olanı alır, mantık hatası barındıran satırlarda “availability_365” değerine eşittir, yani gerçekçi bir üst sınıra çeker. Diğerlerinde orijinal değeri korur.
- “availability_365” değerinin 0 olduğu durumlarda “minimum_nights_adjusted” değeri de 0’a eşitlenir ve bu durum o ilanın kiralanamayacağını gösterir.### Kontrol edilip temiz bulunan sütunlar
- Duplicate satır yok.
- “price” doğru veri tipinde.
- “room_type” sadece 3 tutarlı kategori içeriyor.
- “latitude” ve “longitude” NYC nin coğrafi konumuyla (kuzey batı yarımküre) uyuşuyor.
- “availability_365” her zaman 0-365 aralığında.


## Sonuç
Temizlenmiş veri setinde eksik değer kalmadı. (“data.isnull().sum()” sonucu tüm sütunlarda 0). Silme yerine flagleme ve mantık temelli doldurma tercih edilerek orijinal veri kaybı minimuma indirildi. Böylece ileriki analizlerde şüpheli satırlar kolayca ayrıştırılabilir hale geldi.

## Kullanılan Araçlar
Python, pandas, numpy, Jupyter Notebook




















