# Cafe Sales Data Cleaning

- İçerik: Cafe Sales (Kaggle) data seti üzerinde uçtan uca bir data cleaning çalışması.
- Amaç: Gerçekçi bir kirli veri setinde eksik beri ile karşılaşınca kullanılacak stratejileri pratik etmek.

## Dataset

- Kaynak: [Cafe Sales - Dirty Data for Cleaning Training](https://www.kaggle.com/datasets/ahmedmohamed2003/cafe-sales-dirty-data-for-cleaning-training)

- Toplam: 10.000 satır, 8 sütun


## Bulunan Sorunlar ve Çözümler

### 1. Yanlış Veri Tipi İçeren Satırlar
- Quantity, Price Per Unit, Total Spent satırları sayısal olmaları gerekirken string (object) olarak bulunuyordu. pd.to_numeric() kullanılarak sayıya çevirildi.

### 2. Eksik Veri İçeren Satırlar
- Öncelikle "ERROR" / "UNKNOWN" değerleri np.nan ile değiştirildi. Bu sayede gerçek eksik veri oranı ortaya çıkarılmış oldu.
- Total Spent = Quantity x Price Per Unit ilişkisi kullanılarak üç satırdan herhangi biri eksikse diğer ikisi kullanılarak hesaplandı ve dolduruldu. 
- Payment Method, Location, Transaction Date için diğer sütunlarla istatistiksel bir ilişki bulunamadığı için bu satırlardaki eksik değerler “Unknown” ile dolduruldu.
- Price Per Unit değeri unique olan ürünler için eksik Item isimleri fiyattan tahmin edildi. Fiyatı çakışan ürünler için bu yöntem güvenilir olmadığı için uygulanmadı.
- Birden fazla ilgili sütun aynı anda boş olup hiçbir yöntemle doldurulması mümkün olmayan az sayıda satır (57) silindi.

## Sonuç
- Tüm sütunlarda eksik veri sayısı 0’a indirildi. Temiz veri yeni bir csv dosyası olarak kaydedildi.
- Item sütununun bir kısmı (474 satır) aynı fiyatta birden fazla ürün olması sebebiyle kesin olarak tespit edilemedi, “Unknown” olarak işaretlendi.
- Transaction Date ve Transaction ID arasında kronolojik bir ilişki tespit edilemediği için eksik tarihler üzerine tahmin yapılmadı.

## Kullanılan Araçlar
Python, pandas, numpy, Jupyter Notebook


