# TURKCELL-CodeFem3-VISION-INSIGHT

TURKCELL Geleceği Yazan Kadınlar Yapay Zeka Programı kapsamında, 

"CodeFem3" proje ekibimiz ile geliştirdiğimiz,

"VISION-INSIGHT: Gerçek Zamanlı Görüntü İşleme ile Müşteri Tercih Analizi Sistemi" projemizi size sunmaktayız.

**Proje Ekip Üyeleri:** Pelin Nur ÇÖL, Selay YIRTIMCI, Yağmur BAYRAK

----------------------------------------------------------------------

**Proje Hakkında;**

Market raflarının önüne yerleştirilmiş sabit bir kameradan alınan videolardan yararlanılarak müşterilerin temsili davranışlarının otomatik olarak tanınması hedeflenmiştir. Videolardan tek tek görüntüler çıkarılmış; "Rafa Uzan", "Raftan Geri Çek", "Rafa El Koy", "Ürünü İncele", "Rafı İncele" ve "Arka Plan" gibi altı temel hareket etiketlenmiş ve sistem bu örneklerle kademeli biçimde eğitilmiştir. Verinin bir bölümü eğitim (train), daha küçük bir bölümü doğrulama (validation), en sona ayrılan tamamen farklı bir bölüm ise gerçek hayata benzer bir sınav niteliğinde bağımsız test için kullanılmıştır. Elde edilen modelin ilk tahminindeki doğruluk oranı yaklaşık %28–32 aralığında gözlenmiş; “ilk beş tahminden biri doğru mu?” ölçütünde (Top-5) bu oran %94–95 düzeyinde gerçekleşmiştir. Çalışma, mağaza içinde hangi rafta hangi ürüne daha fazla ilgi gösterildiğinin anlaşılması, yerleşim ve stok kararlarının iyileştirilmesi ve yoğunluk anlarının saptanması gibi uygulamalara temel oluşturmaktadır. Amaç, mahremiyete saygı gözetilerek genel davranışların analiz edilmesi ve bu yolla işletmelere öngörü ile hız kazandırılmasıdır.

----------------------------------------------------------------------

**DATASET ÖN İŞLEME:**

Eğitimden önce, MERL Shopping Dataset arşivleri indirildi ve bütünlük/doğrulama kontrolleri yapılarak veri yapısı çözümlendi; eylem açıklamalarındaki zaman damgaları esas alınarak Reach_To_Shelf, Retract_From_Shelf, Hand_In_Shelf, Inspect_Product, Inspect_Shelf ve Background olmak üzere 6 sınıflık bir şema tanımlandı. Tüm videolar tek biçeme getirildi ve kare çıkarımı 15 FPS’te gerçekleştirildi; eylem aralığına düşen kareler tam örneklenirken, veri dengesini sağlamak için Background sınıfı yaklaşık 1/6 oranında aşağı örneklendi. Üretilen kareler, en-boy oranı korunarak 224×224 hedef boyuta yeniden ölçeklendi (gerekli yerlerde merkez kırpma/padding uygulanarak) ve bozuk/okunamayan kareler elendi; sınıf adları ile sayısal etiketler için tekil bir class-index eşlemesi ve örnek başına meta bilgileri içeren bir manifest CSV oluşturuldu. Kişi sızıntısını engellemek amacıyla denek kimliği bazlı ayrım önceden tanımlanarak 1–20 train, 21–26 val, 27–41 test listeleri üretildi ve dosya düzeni {split}/{class}/SUBJxx_SEQyy_FRAMEzzzzz.jpg olacak şekilde standartlaştırıldı; son olarak, örnek sayıları ve etiket tutarlılığı denetlenip dataset.yaml/CSV ile birlikte paketlenerek arşiv ve paylaşım için Google Drive’a aktarıldı.

----------------------------------------------------------------------

**TRAIN:**

Eğitim aşamasında, MERL Shopping Dataset videolarından 15 FPS hızında çıkarılan karelerle, Reach_To_Shelf, Retract_From_Shelf, Hand_In_Shelf, Inspect_Product, Inspect_Shelf ve Background olmak üzere 6 sınıflı bir görüntü-sınıflandırma kümesi oluşturuldu; kişi (subject) sızıntısını önlemek amacıyla subject-bazlı ayrım uygulanarak 1–20 numaralı denekler train, 21–26 val, 27–41 test olarak ayrıldı ve eylem aralığı dışında kalan arka plan kareleri her 6 karede 1 tutuldu. Eğitim, Ultralytics’in sınıflandırma modeli yolo11n-cls ile 224×224 giriş boyutu, batch=32, epoch=60 ve early stopping (patience=10) etkin olacak şekilde yürütüldü; AdamW optimizasyonu (başlangıç öğrenme oranı 5×10⁻⁴), mixup=0 ve cutmix=0, AMP kapalı, seed=0 ve device=0 (GPU) ayarları kullanıldı. Eğitim çıktılarına ilişkin günlükler yerel runs klasörüne yazılırken en iyi ağırlık (best.pt), son ağırlık (last.pt) ve metrik kayıtları (results.csv) güvenli kopyalama ile Google Drive’a aktarılıp arşivlendi.

----------------------------------------------------------------------

**TRAIN, VALIDATION, TEST SONUÇLARI:**

Eğitim sonucunda EarlyStopping (patience=10) doğrulama kaybındaki iyileşmenin durmasıyla tetiklenmiş ve süreç 12. epoch’ta sonlandırılmış; en düşük doğrulama kaybını veren model ağırlıkları 2. epoch’ta elde edilerek best.pt adıyla kaydedilmiştir. Train aşamasında kayıp (loss) düzenli biçimde azalmış, doğruluk metrikleri doğrulama eğilimleriyle uyumlu bir yakınsama göstermiştir (ayrıntılı sayısal değerler "EXPERIMENTAL-RESULTS" dosyasında results.csv’de yer almaktadır). Bu en iyi modelle Validation kümesinde Top-1 doğruluk %31,5, Top-5 doğruluk %95,1; bağımsız Test kümesinde ise Top-1 doğruluk %27,62, Top-5 doğruluk %94,11 olarak raporlanmıştır.

----------------------------------------------------------------------

**CLASSES:**

"0": "Background"

"1": "Reach_To_Shelf"

"2": "Retract_From_Shelf"

"3": "Hand_In_Shelf"

"4": "Inspect_Product"

"5": "Inspect_Shelf"

Background (Arka Plan): Raf ya da ürünle belirgin bir etkileşimin olmadığı genel sahneler.

Reach_To_Shelf (Rafa Uzanma): Müşterinin elini raftaki ürüne doğru uzattığı, temas öncesi yaklaşma anı.

Retract_From_Shelf (Raftan Geri Çekilme): Elin raftan uzaklaştığı, ürünü aldıktan ya da almadan geri çekildiği an.

Hand_In_Shelf (Elin Raf İçinde): Elin raf bölmesinin içinde bulunduğu, ürüne fiziksel temas/yerleştirme yapılan anlar.

Inspect_Product (Ürünü İnceleme): Ürünün elde tutulup ambalaj/etiket gibi ayrıntıların incelendiği yakın bakış anı.

Inspect_Shelf (Rafı İnceleme): Ürün elde değilken, raf üzerindeki ürünlerin gözle tarandığı/arındığı an.

----------------------------------------------------------------------

**DATASET:**

Model eğitiminde kullanılan veriseti;

*MERL SHOPPING DATASET*: https://www.merl.com/research/downloads/MERL_Shopping_Dataset

*MERL SHOPPING DATASET DOWNLOAD LINK:* https://www.merl.com/pub/tmarks/MERL_Shopping_Dataset/

----------------------------------------------------------------------

**LINKS:**

PROJECT GOOGLE DRIVE: https://drive.google.com/drive/u/0/folders/1t80qNHSfx3cm5a_S7P-cWQaQV7ibpua-

DATASET LABEL FILES: https://drive.google.com/drive/u/0/folders/1SN3x6wkf0sLEhTejimXQgH9VmSwek2rA

DATASET VIDEO FILES: https://drive.google.com/drive/u/0/folders/1pDq_SOveelxqY88uyIh1bUCHVvL5qMmw

TRAINED-MODEL FILE (best.pt): https://drive.google.com/drive/u/0/folders/1WSX0ECpfDUhXg_01nBhTn91TfuvRDgpG

----------------------------------------------------------------------

***!!! IMPORTANT !!! IMPORTANT !!! IMPORTANT !!!***
   
**WEB SITE UYGULAMASINDA EĞİTİLMİŞ MODELİ TEST ETME ADIMLARI:**

*(Not: Tüm Kullanıcılar İçin Kullanımı Uygundur.)*

1. WEB APP LINK: https://codefem3.site

2. ALTERNATİF WEB APP LINK: https://vision-insight-728185266329.europe-west1.run.app

3. Her iki link de aynı web uygulamasını açar.

4. Uygulama sizden boyutu çok büyük olmayacak şekilde .mp4 uzantılı dosya yüklemenizi istiyor. (tercihen < 32 MB)

5. Uygulamayı test ederken kullanabileceğiniz örnek test videolarına ilgili linkten ulaşabilirsiniz: https://drive.google.com/drive/u/0/folders/1aKwxQPHdhnqISGHIaQtndQckios13pOr

6. Seçtiğiniz bir dosyayı bilgisayarınıza indiriniz. (Not: Testin hızlı bitmesini isterseniz "mini.mp4" adlı dosya ile teste başlayabilirsiniz.)

7. Ardından email adresinizi giriniz. Sonuçları içeren .zip dosyasını emailinize gönderecektir. Fakat bu özellik şu anda aktif değildir.

8. "Testi Başlat" isimli butona basınız.

9. Test uzun sürebilir. Endişelenmeyiniz. Videonuzun büyüklüğüne bağlı olarak test süresi 15-30 dakikaya kadar çıkabilir. Lütfen sekmeyi kapatmayınız.

10. Web uygulama sonuçları içeren .zip dosyasını otomatik olarak bilgisayarınıza indirecektir ve ekranda buton altında "Sonuç ZIP indirildi." bilgilendirme mesajı çıkacaktır.

11. Bilgisayarınızda "İndirilenler" klasörünüze bakınız. "VIDEO_NAME_results.zip" dosyasını göreceksiniz (VIDEO_NAME yüklediğiniz dosyanın ismidir).

12. İçerisinde her bir karesi bir sınıfa etiketlenmiş .mp4 videosu ve sonuç analiz dosyaları bulunacaktır.

13. Uygulamamızı test ettiğiniz için teşekkür ederiz.

**WEB APP DEMO VIDEO:** 

----------------------------------------------------------------------   

**GOOGLE COLAB'DE EĞİTİLMİŞ MODELİ TEST ETME ADIMLARI:**

*(Not: Yazılım Geliştirme Uzmanları İçin Kullanımı Uygundur.)*

1. EĞİTİLMİŞ MODEL: best.pt ("TRAINED-MODEL" klasörü altında bulunmaktadır.). "best.pt" dosyasını indiriniz.

2. "TEST-VIDEO" klasörü altındaki "29_2_crop.mp4" dosyasını indiriniz.

    Farklı bir video ile test yapmak isterseniz;

    VIDEO FILES: https://drive.google.com/drive/u/0/folders/1pDq_SOveelxqY88uyIh1bUCHVvL5qMmw

    Google Drive linki ile .mp4 uzantılı dosyalara ulaşabilmektesiniz.

3. "TURKCELL_CodeFem3_VISION_INSIGHT_PROJECT_TEST_THE_TRAINED_MODEL.ipynb" isimli dosyayı Google Colab platformunda açınız.

4. Seçtiğiniz ".mp4" dosyasını ve "best.pt" model dosyasını Google Colab'e yükleyiniz.

5. Çalışma Türü; "A100 GPU" veya "L4 GPU" seçiniz.

6. Kodun içersinde video ismini seçtiğiniz video ismi ile düzeltiniz.

7. RUN CODE: Kodunuzu çalıştırabilirsiniz.
   
8. Test sonuçlarını alacaksınız.

**EXAMPLE OF VIDEO TESTING RESULTS:**  https://drive.google.com/drive/u/0/folders/16cIH5difla53LjKoxAtv0cofWl35Aqlh

**TEST DEMO VIDEO:** https://drive.google.com/drive/u/0/folders/1iNNIQ8YSQnesq1CEiTY63dHIdm2K-1b3

----------------------------------------------------------------------


