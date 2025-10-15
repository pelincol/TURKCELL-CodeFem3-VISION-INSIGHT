# TURKCELL-CodeFem3-VISION-INSIGHT

TURKCELL Geleceği Yazan Kadınlar Yapay Zeka Programı kapsamında, 

"CodeFem3" proje ekibimiz ile geliştirdiğimiz,

"VISION-INSIGHT: Gerçek Zamanlı Görüntü İşleme ile Müşteri Tercih Analizi Sistemi" projemizi size sunmaktayız.

**Proje Ekip Üyeleri:** Pelin Nur ÇÖL, Selay YIRTIMCI, Yağmur BAYRAK

----------------------------------------------------------------------

PROJECT GOOGLE DRIVE: https://drive.google.com/drive/u/0/folders/1t80qNHSfx3cm5a_S7P-cWQaQV7ibpua-

DATASET LABEL FILES: https://drive.google.com/drive/u/0/folders/1SN3x6wkf0sLEhTejimXQgH9VmSwek2rA

DATASET VIDEO FILES: https://drive.google.com/drive/u/0/folders/1pDq_SOveelxqY88uyIh1bUCHVvL5qMmw

TRAINED-MODEL FILE (best.pt): https://drive.google.com/drive/u/0/folders/1WSX0ECpfDUhXg_01nBhTn91TfuvRDgpG

**!!! EXAMPLE OF VIDEO TESTING RESULTS:** https://drive.google.com/drive/u/0/folders/16cIH5difla53LjKoxAtv0cofWl35Aqlh

----------------------------------------------------------------------

EĞİTİLMİŞ MODELİ TEST ETME ADIMLARI:

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
  
   4 farklı örnek test sonuçları: https://drive.google.com/drive/u/0/folders/16cIH5difla53LjKoxAtv0cofWl35Aqlh

----------------------------------------------------------------------

CLASSES:

"0": "Background"

"1": "Reach_To_Shelf"

"2": "Retract_From_Shelf"

"3": "Hand_In_Shelf"

"4": "Inspect_Product"

"5": "Inspect_Shelf"

