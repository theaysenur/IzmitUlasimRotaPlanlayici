# İzmit Şehir İçi Ulaşım Rota Planlayıcısı 🚌🚊

![Java](https://img.shields.io/badge/Java-17+-ED8B00?style=flat&logo=openjdk&logoColor=white)
![JavaFX](https://img.shields.io/badge/JavaFX-GUI-5382A1?style=flat&logo=java&logoColor=white)
![Maven](https://img.shields.io/badge/Maven-Build-C71A36?style=flat&logo=apachemaven&logoColor=white)

İzmit ilçesi toplu taşıma ağı üzerinde otobüs, tramvay ve taksi hatlarını modelleyerek iki nokta arasındaki en uygun ulaşım rotasını hesaplayan graf tabanlı bir rota planlama sistemidir.

## Proje Hakkında

Bu proje, İzmit'in gerçek koordinatları üzerinde konumlandırılmış otobüs ve tramvay duraklarından oluşan bir ulaşım ağını graf yapısı ile modellemektedir. Kullanıcının başlangıç ve hedef konumlarına en yakın duraklar Haversine formülü ile bulunur, ardından DFS algoritması ile tüm alternatif rotalar hesaplanarak ücret, süre ve mesafe karşılaştırması yapılır. Sistem, otobüs-tramvay aktarma noktalarını ve taksi alternatifini de desteklemektedir.

**Kocaeli Üniversitesi Bilgisayar Mühendisliği — Yazılım Laboratuvarı I (PROLAB I) dersi kapsamında geliştirilmiştir.**

## ✨ Özellikler

- **Graf Tabanlı Ulaşım Ağı:** JSON veri dosyasından okunan duraklar, bağlantılar ve transfer noktaları ile İzmit'in toplu taşıma ağı modellenir. Her durak enlem/boylam koordinatlarına sahiptir.
- **DFS ile Alternatif Rota Bulma:** Başlangıç ve hedef durak arasındaki tüm olası rotalar DFS (Depth-First Search) algoritması ile keşfedilir. Döngü engelleme mekanizması ile sonsuz döngüler önlenir.
- **Haversine Mesafe Hesaplama:** Gerçek enlem/boylam koordinatları kullanılarak iki nokta arasındaki kuş uçuşu mesafe hesaplanır. Kullanıcıya en yakın durak bu yöntemle belirlenir.
- **Otobüs ↔ Tramvay Transferi:** Farklı ulaşım türleri arasında aktarma desteği sağlanır. Transfer süresi ve ücreti ayrıca hesaplanır.
- **Taksi Entegrasyonu:** Toplu taşıma rotası bulunamadığında veya alternatif olarak taksi seçeneği sunulur. Açılış ücreti + km başına maliyet üzerinden fiyatlandırılır.
- **Çoklu Ödeme Yöntemi (Strategy Pattern):** Nakit, KentKart ve kredi kartı ödeme seçenekleri `Odeme` arayüzü üzerinden Strategy Pattern ile implement edilmiştir.
- **Yolcu Türleri ve İndirim (Polymorphism):** Öğrenci yolcular %50, yaşlı yolcular %30 indirimden yararlanır. `Yolcu` abstract sınıfı ve `getIndirimOrani()` polimorfizmi ile yönetilir.
- **Rota Metrikleri:** Her alternatif rota için toplam ücret (₺), toplam süre (dk) ve toplam mesafe (km) ayrıntılı olarak hesaplanır ve karşılaştırılır.
- **JavaFX Harita Arayüzü:** Duraklar ve rotalar interaktif harita üzerinde görselleştirilir.

## 🛠️ Teknoloji Yığını

| Katman | Teknoloji |
|--------|-----------|
| Dil | Java 17+ |
| Yapı Aracı | Maven |
| Arayüz | JavaFX |
| Veri İşleme | Gson (JSON parsing) |
| Algoritmalar | DFS, Haversine Formula |
| Tasarım Desenleri | Strategy Pattern, Polymorphism, Abstraction |
| Test | JUnit 5 |

## 🏗️ Sistem Mimarisi

Proje dört temel katmandan oluşur:

**Veri Katmanı:** `jsonveri.txt` dosyasından İzmit'in otobüs ve tramvay durakları, bağlantıları ve taksi bilgileri `JSONVeriYukleyici` ile okunarak `SehirVerisi` nesnesine dönüştürülür. Her durak; ID, isim, tür (bus/tram), koordinat, sonraki duraklar ve transfer bilgisi içerir.

**Graf ve Algoritma Katmanı:** `Graph` sınıfı durak listesini yönetir ve en yakın durak bulma işlemini gerçekleştirir. `RotaPlanlayici` DFS ile tüm alternatif rotaları keşfeder. `CustomRotaPlanlayici` Haversine tabanlı mesafe hesaplamasıyla optimize edilmiş rotalar sunar. Her rota için ücret, süre ve mesafe metrikleri hesaplanır.

**İş Mantığı Katmanı:** `Yolcu` hiyerarşisi (Öğrenci, Yaşlı) indirim oranlarını polimorfizm ile yönetir. `Odeme` arayüzü (Nakit, KentKart, KrediKartı) Strategy Pattern ile ödeme işlemlerini gerçekleştirir. `Arac` hiyerarşisi (Otobus, Tramvay, TaksiArac) araç türlerini modelller.

**Sunum Katmanı:** `MapArayuzFX` JavaFX ile interaktif harita arayüzü sunar. Duraklar ve rotalar harita üzerinde görselleştirilir.

## 📁 Proje Yapısı

```
prolab/
├── .gitignore
├── README.md
├── pom.xml
└── src/
    └── main/
        ├── java/org/kocaeli/ulasim/
        │   ├── Main.java                    # Ana giriş noktası
        │   ├── DemoUygulamasi.java          # Demo senaryoları
        │   ├── Graph.java                   # Graf yapısı ve durak yönetimi
        │   ├── RotaPlanlayici.java          # DFS ile rota bulma
        │   ├── CustomRotaPlanlayici.java    # Optimize edilmiş rota planlama
        │   ├── Durak.java                   # Durak veri modeli
        │   ├── NextStop.java                # Sonraki durak bağlantısı
        │   ├── Transfer.java                # Aktarma bilgisi
        │   ├── Konum.java                   # Enlem/boylam koordinatı
        │   ├── Arac.java                    # Abstract araç sınıfı
        │   ├── Otobus.java                  # Otobüs alt sınıfı
        │   ├── Tramvay.java                 # Tramvay alt sınıfı
        │   ├── TaksiArac.java               # Taksi aracı alt sınıfı
        │   ├── Taksi.java                   # Taksi ücretlendirme
        │   ├── Yolcu.java                   # Abstract yolcu sınıfı
        │   ├── OgrenciYolcu.java            # Öğrenci yolcu (%50 indirim)
        │   ├── YasliYolcu.java              # Yaşlı yolcu (%30 indirim)
        │   ├── Odeme.java                   # Ödeme arayüzü (Strategy)
        │   ├── NakitOdeme.java              # Nakit ödeme
        │   ├── KentKartOdeme.java           # KentKart ödeme
        │   ├── KrediKartiOdeme.java         # Kredi kartı ödeme
        │   ├── SehirVerisi.java             # Şehir veri modeli
        │   ├── JSONVeriYukleyici.java        # JSON veri okuyucu
        │   ├── DistanceCalculator.java      # Mesafe hesaplama arayüzü
        │   ├── HaversineDistanceCalculator.java  # Haversine implementasyonu
        │   ├── HaritaGosterici.java         # Harita gösterim arayüzü
        │   └── MapArayuzFX.java             # JavaFX harita arayüzü
        └── resources/
            ├── jsonveri.txt                 # İzmit ulaşım ağı verisi
            ├── durak.jpg                    # Durak ikonu
            └── tramvay.jpg                  # Tramvay görseli
```

## 🚀 Çalıştırma

```bash
# Repoyu klonla
git clone https://github.com/theaysenur/prolab.git
cd prolab

# Maven ile derle ve çalıştır
mvn clean compile exec:java -Dexec.mainClass="org.kocaeli.ulasim.Main"
```

> **Not:** JavaFX modüllerinin sistemde kurulu olması gerekir.

## 📚 Kullanılan OOP Kavramları ve Tasarım Desenleri

| Kavram / Desen | Uygulama |
|----------------|----------|
| **Kalıtım (Inheritance)** | `Yolcu` → `OgrenciYolcu`, `YasliYolcu`; `Arac` → `Otobus`, `Tramvay`, `TaksiArac` |
| **Soyutlama (Abstraction)** | `Yolcu`, `Arac` abstract sınıfları |
| **Çok Biçimlilik (Polymorphism)** | `getIndirimOrani()` her yolcu türünde farklı davranış |
| **Kapsülleme (Encapsulation)** | Tüm model sınıflarında private alanlar + getter/setter |
| **Strategy Pattern** | `Odeme` interface → `NakitOdeme`, `KentKartOdeme`, `KrediKartiOdeme` |
| **Dependency Injection** | `DistanceCalculator` interface ile mesafe hesaplama stratejisi enjeksiyonu |

## 📄 Lisans

Bu proje eğitim amaçlı geliştirilmiştir.
