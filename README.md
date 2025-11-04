# Kaggle Sample Sales Data - Polars ile Veri Analizi Projesi

Bu proje, Kaggle'ın "Sample Sales Data" (Örnek Satış Verileri) veri setini kullanarak, modern ve yüksek performanslı `polars` kütüphanesi ile uçtan uca bir keşifçi veri analizi (EDA) çalışmasıdır. Projenin amacı, ham veriden yola çıkarak temizleme, zenginleştirme (feature engineering) ve görselleştirme adımlarını uygulamak ve işlenebilir içgörüler (actionable insights) elde etmektir.

Bu analiz, sadece veriyi özetlemekle kalmayıp, veri setindeki gizli bir **iş kuralını** (fiyatlandırma ve kâr marjı) ortaya çıkarmaya odaklanmıştır.

## 🚀 Kullanılan Teknolojiler

  * **Veri Manipülasyonu ve Analizi:** `polars`
  * **Görselleştirme:** `matplotlib` & `seaborn`
  * **Veri Dönüşümü:** `pandas` (Görselleştirme kütüphaneleriyle uyumluluk için)
  * **Çalışma Ortamı:** `Jupyter Lab`

## 🛠️ Analiz Metodolojisi

Proje, profesyonel bir veri bilimi iş akışını takip ederek 5 ana adıma ayrılmıştır:

1.  **Veri Yükleme ve İlk Bakış:** `polars`'ın `read_csv` fonksiyonu ile veri yüklendi ve `schema` (şema) analizi yapıldı.
2.  **Veri Temizliği ve Dönüşüm:**
      * `ORDERDATE` (Sipariş Tarihi) sütunu, metinden (`String`) Polars'ın `Datetime` formatına dönüştürüldü.
      * Verimlilik için `STATUS`, `PRODUCTLINE` gibi sık tekrar eden metin sütunları `Categorical` (Kategorik) tipe çevrildi.
      * Gereksiz (`ADDRESSLINE2`, `PHONE` vb.) ve analize katkısı olmayan sütunlar kaldırıldı.
3.  **İçgörü Keşfi (Hipotez Testi):**
      * `SALES` (Satışlar) sütununun, `QUANTITYORDERED * PRICEEACH` (Miktar \* Birim Fiyat) formülünden saptığı gözlemlendi.
      * Bu sapmanın bir "hata" değil, "Large" ve "Medium" olarak işaretlenen anlaşmalarda uygulanan bir **kâr marjı (markup)** olduğu tespit edildi.
4.  **Özellik Mühendisliği (Feature Engineering):**
      * `ORDERDATE` sütunundan `ORDER_YEAR`, `ORDER_MONTH`, `ORDER_WEEKDAY` gibi yeni analiz sütunları türetildi.
      * Keşfedilen kâr marjını ölçmek için `CALCULATED_SALES`, `MARKUP_AMOUNT` (Kâr Tutarı) ve `MARKUP_PERCENTAGE` (Kâr Yüzdesi) sütunları oluşturuldu.
5.  **Keşifçi Veri Analizi (EDA) ve Görselleştirme:** Temel iş sorularını yanıtlamak için `group_by` ve `agg` işlemleri kullanıldı ve sonuçlar görselleştirildi.

-----

## 📈 Ana Bulgular ve İçgörüler

Analiz sonucunda 4 temel iş sorusuna yanıt bulundu:

### 1\. BulgU: Fiyatlandırmanın Arkasındaki İş Kuralı Çözüldü

Veri seti, `DEALSIZE` (Anlaşma Büyüklüğü) ile belirlenen net bir kâr marjı (markup) modelini ortaya koydu. `PRICEEACH` taban fiyatı temsil ederken, `SALES` kârlı nihai fiyatı göstermektedir.

  * **Large (Büyük) Anlaşmalar:** Taban fiyat üzerine ortalama **%80.8 kâr** marjı uygulanır.
  * **Medium (Orta) Anlaşmalar:** Ortalama **%24.7 kâr** marjı uygulanır.
  * **Small (Küçük) Anlaşmalar:** Neredeyse sıfır kârla (%1.9) taban fiyattan satılır.

 \#\#\# 2. BulgU: "Classic Cars" Kategorisi Pazarı Domine Ediyor

Ciroya göre en güçlü ürün grubu açık ara **Classic Cars (Klasik Arabalar)** olup, toplamda yaklaşık 3.9 Milyon $ciro üretmiştir. Bu, en yakın rakibi olan Vintage Cars (1.9M$) kategorisinin iki katından fazladır.

 \#\#\# 3. BulgU: ABD Pazarı, Cironun Ana Kaynağı

Coğrafi olarak, cironun ezici bir çoğunluğu (3.6M $) **ABD** pazarından gelmektedir. İkinci sırada İspanya (1.2M $) ve üçüncü sırada Fransa (1.1M $) bulunmaktadır.

 \#\#\# 4. BulgU: Satış Trendi (ve 2005 Veri Uyarısı)

Yıllık satışlar 2003'ten 2004'e önemli bir artış göstermiştir. Ancak 2005 yılındaki keskin düşüş, veri setinin 2005 yılının tamamını değil, yalnızca bir kısmını (muhtemelen ilk çeyrek veya ilk 5 ay) içerdiğini göstermektedir. Bu nedenle yıllık karşılaştırmalarda 2005 yılı dikkatli yorumlanmalıdır.

-----

## 🔧 Projeyi Çalıştırma

Bu analizi kendi makinenizde yeniden oluşturmak için:

1.  Depoyu klonlayın:

    ```bash
    git clone https://github.com/KULLANICI_ADINIZ/REPO_ADINIZ.git
    cd REPO_ADINIZ
    ```

2.  `sales_data_sample.csv` dosyasını ana proje klasörüne indirin.

3.  Bir sanal ortam (virtual environment) oluşturun ve aktive edin:

    ```bash
    python -m venv venv
    # Windows
    .\venv\Scripts\Activate
    # macOS/Linux
    source venv/bin/activate
    ```

4.  Gerekli kütüphaneleri yükleyin (bir `requirements.txt` dosyanız varsa daha iyi olur):

    ```bash
    pip install polars pandas matplotlib seaborn jupyterlab
    ```

5.  Jupyter Lab'ı başlatın ve analiz notebook'unu açın:

    ```bash
    jupyter lab
    ```