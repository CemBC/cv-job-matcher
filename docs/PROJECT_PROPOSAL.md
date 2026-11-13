# Kısa proje önerisi — araştırma taslağı (henüz veri kaynağı kesinleşmedi)

## Başlık
Türkçe Özgeçmişler ile İş İlanlarının Doğal Dil İşleme Yöntemleriyle Eşleştirilmesi ve Uygunluk Sıralaması

## Problem
Aynı Türkçe yazılım geliştirici ilanına çok sayıda özgeçmiş geldiğinde beceri ve deneyim bilgisinin elle karşılaştırılması zaman alır. Birebir anahtar kelime karşılaştırması, farklı ifadelerle anlatılan aynı becerileri kaçırabilir. Adayları insan denetimini koruyarak sıralayan ve kullanılan metinsel kanıtları açıklayan ölçülebilir bir yönteme ihtiyaç vardır.

## Tek amaç
**Türkçe yazılım geliştirici ilanlarına başvuran özgeçmişleri, ilanla metinsel ve beceri temelli uyumlarına göre açıklanabilir biçimde sıralayan ve farklı NLP yöntemlerinin sıralama başarısını karşılaştıran bir prototip geliştirmek.**

## Somut hedefler
1. Kullanımı izinli/anonymize edilmiş Türkçe özgeçmiş ve yazılım geliştirici iş ilanı koleksiyonu oluşturmak; sayıları gerçek erişim sonrası raporlamak.
2. İlan ve CV'lerden yalnızca iş için ilgili teknik beceriler, deneyim ve eğitim gereksinimlerini (gerekiyorsa) çıkarmak; isim, iletişim, fotoğraf, yaş ve diğer hassas nitelikleri kullanmamak.
3. **TF-IDF + cosine** baseline ile **Türkçe/çok dilli sentence embedding + cosine** yöntemlerini aynı veri üzerinde karşılaştırmak.
4. Açık gereksinimler ile CV'de tespit edilen beceriler için şeffaf, ölçülü kapsam puanı ve eksik kanıt listesi üretmek.
5. İnsanların değerlendirdiği, ayrı tutulmuş eşleşme/ranking test seti üzerinde NDCG@k ve Precision@k gibi metrikleri raporlamak; hata analizi yapmak.
6. Tek ilan–çoklu CV sıralaması, skor açıklaması ve yerel kayıtları olan localhost prototipi sunmak.

## Literatür başlangıç noktaları (özeti doğrulanmış; tam metinler ayrıca incelenecek)
- Kaya & Bogers (2023), *An Exploration of Sentence-Pair Classification for Algorithmic Recruiting*, RecSys 2023, DOI: 10.1145/3604915.3610657 — Sentence-BERT ile BERT tabanlı çift sınıflandırma yaklaşımlarını karşılaştırır.
- Vanetik & Kogan (2023), *Job Vacancy Ranking with Sentence Embeddings, Keywords, and Named Entities*, Information 14(8), 468, DOI: 10.3390/info14080468 — embeddings, anahtar kelime ve varlıkların birlikte kullanımını tartışır.
- Deshmukh & Raut (2025), *Applying BERT-Based NLP for Automated Resume Screening and Candidate Ranking*, Annals of Data Science 12, 591–603, DOI: 10.1007/s40745-024-00524-5 — BERT tabanlı özgeçmiş ön eleme ve sıralamayı ele alır.
Bu literatür kaynakları **Türkçe veri seti doğrulaması değildir**; APA 7 bilgileri final rapordan önce yeniden kontrol edilmelidir.

## Veri kümesi ve erişim
- **CV:** gönüllü katılımcılardan açık rıza ve anonimleştirme ile temin edilen, yazılım geliştirici alanıyla sınırlı Türkçe özgeçmişler; uygun lisanslı akademik veri varsa alternatif. **Henüz doğrulanmış CV kümesi yok.**
- **İlan:** kullanım koşullarının izin verdiği açık iş ilanları, kurumların kendi yayımladığı ve araştırmada işlenmesine izin verilen ilanlar veya izinli API/veri setleri. Kaynak ve kayıt adedi doğrulanmadan kesin söylenmeyecek. Scraping ancak kaynak özelinde izin, kullanım şartı, robots ve KVKK değerlendirmesi sonrası.
- **Veri alanları:** gerekli/tercih edilen beceriler; ilana bağlı nitelikler; CV'de açıkça yazan teknik beceriler, deneyim ifadeleri ve ilgili eğitim/sertifikalar. Kişisel tanımlayıcılar sıralamada kullanılmayacak.
- **Etiketleme:** bir ilana ilişkin birden çok CV için değerlendiricilerin 0=uyumsuz, 1=kısmen uyumlu, 2=uyumlu etiketleri vermesi önerilir; net ölçek ve sayılar pilot etiketleme ile teyit edilecek.

## Önerilen yöntem ve puan hesabı
- Ortak Türkçe ön işleme → CV/ilanın ilgili metin alanları → iki bağımsız modelin cosine benzerliği.
- **TF-IDF skoru**: cosine(tfidf(ilan), tfidf(CV)); **embedding skoru**: cosine(embedding(ilan), embedding(CV)). Bunlar 0–100 aday uygunluk yüzdeleri değildir; ham skor olarak gösterilir, seçili modele göre sıralama yapılır.
- **Beceri kapsamı (ayrı açıklayıcı metrik)**: `tespit_edilen_zorunlu_beceriler / ilandaki_zorunlu_beceriler` (zorunlu beceri yoksa N/A). Eşanlamlar/alias sözlüğü ve kanıt metinleriyle desteklenir. Beceri puanı model skoruna ancak ayrıca etiketli veri üzerinde geçerli bir ağırlık doğrulanırsa eklenir.
- **Kritik gereksinimler ve hata**: olumsuzluk, farklı deneyim düzeyleri ve ifade varyasyonları için manuel hata analizi. CV'de bulunmamak becerinin yokluğunun kanıtı değildir.

## Sistem mimarisi (kodlama sonraki aşama)
React/TypeScript (localhost:5173) → FastAPI (localhost:8000) → metin çıkarımı/ön işleme → TF-IDF & embeddings → açıklanabilir sıralama → SQLite (yerel). PDF/DOCX yükleme ve sonuç ekranı.

## Başarı ölçütleri
Etiketli **ilan bazlı aday listeleri** üzerinde NDCG@5 (birincil) ve Precision@5 (yardımcı; ilgili sayılan etiket eşiği açıkça tanımlanacak). CV ve ilan bazında veri sızıntısını engelleyen ayırma, sabit test kümesi ve örnek hata analizi zorunlu. Yalnızca aday benzerlik skoru bir başarı metriği değildir.

## Beklenen çıktılar
Denetlenebilir Türkçe veri kaynağı / veri sözlüğü ve etiketleme protokolü; literatür ve yöntem karşılaştırması; aynı ilan için çoklu CV sıralaması yapan lokal web prototipi; eşleşen ve metinde bulunmayan beceriler; deney sonuç tabloları/grafikleri; **Analiz Raporu** ve **Gerçekleştirim Raporu**.

> Durum: **ÖN TASLAK**. Hoca için nihai sürümden önce CV/ilan erişim izinleri, kayıt sayıları, meslek alt kapsamı, literatür künyeleri ve değerlendirme seti doğrulanmalı.
