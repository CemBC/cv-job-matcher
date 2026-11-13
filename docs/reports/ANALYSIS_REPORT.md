# Analiz Raporu

## Kapak Sayfası

**Proje:** Türkçe Özgeçmişler ile İş İlanlarının Doğal Dil İşleme Yöntemleriyle Eşleştirilmesi ve Uygunluk Sıralaması  
**Rapor türü:** Analiz Raporu — planlanan çalışma  
**Durum:** Proje önerisi ve araştırma planı; uygulama, veri toplama ve deney sonuçları henüz gerçekleştirilmemiştir.  
**Hazırlayan:** [Öğrenci adı eklenecek]  
**Kurum / ders / danışman:** [Teslim öncesinde eklenecek]  
**Tarih:** [Teslim tarihinde eklenecek]

> DOCX/PDF teslimine dönüştürülürken özgün öğretmen yönergesindeki A4, Times New Roman 12 punto, ana başlıklarda 14 punto kalın, alt başlıklarda 12 punto kalın, 1,15 satır aralığı, iki yana yaslama ve sol 3 cm/diğer kenarlar 2,5 cm ölçüleri uygulanmalıdır. Üniversitenin resmi biçim kuralları önceliklidir. Bu Markdown kaynak belgesi sayfa düzeni biçimini uygulamaz.

## Özet

Bu proje, Türkçe junior ve mid-level backend yazılım geliştirici iş ilanlarını birden fazla Türkçe özgeçmişle karşılaştıran, sonuçları metinsel kanıtla açıklayan bir araştırma prototipi geliştirmeyi planlar. İki yöntem karşılaştırılacaktır: kelime ve karakter TF-IDF temsilleri üzerinde cosine benzerliği ile çok dilli cümle gömmeleri üzerinde cosine benzerliği. Zorunlu teknik beceri kapsamı ayrı bir kural tabanlı açıklama olarak gösterilecek, model skorlarına eklenmeyecektir. Değerlendirme, izinli veri sağlanması durumunda iki bağımsız değerlendiricinin 0/1/2 metinsel ilgi yargılarına dayanır. NDCG@5 yalnızca her ilan için sabit ve tam yargılanmış en az beş aday ve pozitif ideal kazanç varsa hesaplanacaktır. Ön araştırmada bu proje için erişimi ve yeniden kullanım hakları doğrulanmış Türkçe CV–ilan eşleştirme veri kümesi bulunmamıştır. Bu nedenle öncelikli veri yolu işveren izni ve gönüllü CV katılımıdır; izinler ve kurumsal süreçler doğrulanmadan kişisel veri toplanmayacaktır. Beklenen ürün, yerel React/FastAPI/SQLite prototipi, veri sözlüğü, tekrarlanabilir iki sıralama hattı ve yalnızca veri yeterliyse karşılaştırmalı değerlendirmedir. Bu rapordaki tüm geliştirme ve deneyler planlanmıştır; tamamlanmış uygulama veya sonuç bildirilmemektedir.

## İçindekiler

1. [GİRİŞ](#1-giriş)
   - [1.1 Problem Tanımı](#11-problem-tanımı)
   - [1.2 Projenin Amacı ve Hedefleri](#12-projenin-amacı-ve-hedefleri)
   - [1.3 Projenin Kapsamı](#13-projenin-kapsamı)
2. [LİTERATÜR VE MEVCUT ÇALIŞMALAR](#2-literatür-ve-mevcut-çalışmalar)
   - [2.1 Konuyla İlgili Çalışmalar](#21-konuyla-ilgili-çalışmalar)
   - [2.2 Mevcut Yaklaşımların Değerlendirilmesi](#22-mevcut-yaklaşımların-değerlendirilmesi)
3. [ÖNERİLEN ÇÖZÜM VE YÖNTEM](#3-önerilen-çözüm-ve-yöntem)
   - [3.1 Önerilen Yaklaşım](#31-önerilen-yaklaşım)
   - [3.2 Kullanılacak Yöntem ve Teknolojiler](#32-kullanılacak-yöntem-ve-teknolojiler)
   - [3.3 Sistem Tasarımı veya Çalışma Süreci](#33-sistem-tasarımı-veya-çalışma-süreci)
4. [PROJE PLANI](#4-proje-planı)
   - [4.1 Yapılacak Çalışmalar](#41-yapılacak-çalışmalar)
   - [4.2 Beklenen Çıktılar](#42-beklenen-çıktılar)
5. [SONUÇ](#5-sonuç)
6. [KAYNAKÇA](#6-kaynakça)

---

# 1. GİRİŞ

## 1.1 Problem Tanımı

Bir backend yazılım geliştirici ilanına gelen birden fazla özgeçmişteki beceri ve deneyim kanıtlarını karşılaştırmak zaman alabilir. Basit sözcük örtüşmesi, aynı teknik becerinin farklı adlarını, yazımlarını veya Türkçe ek almış biçimlerini kaçırabilir. Cümle gömme yöntemleri bağlamsal benzerliği yakalamayı amaçlar; ancak benzer metin, kişinin beceriyi gerçekten uyguladığına, başarılı olacağına veya işe alınması gerektiğine kanıt değildir. Bu nedenle araştırma sorusu işe alım kararı üretmek değil, sabit bir ilan için CV metinlerini iki farklı gösterim yöntemiyle sıralayıp bu sıralamaları bağımsız, kanıta dayalı insan metin-ilgi yargılarıyla karşılaştırmaktır.

Temel araştırma engeli veri erişimidir. Kamuya açık ilan metni otomatik olarak yeniden kullanım izni vermez; CV’ler kişisel bilgi içerir. İncelenen aday kaynaklarda Türkçe backend ilanlarıyla eşleştirilmiş, izinleri ve bağımsız insan etiketleri doğrulanmış bir çoklu-CV veri kümesi bulunmamıştır. Dolayısıyla deneysel karşılaştırma, ancak yasal kullanım hakları ve kurumun gerekli gizlilik/araştırma süreci sağlanırsa yürütülebilir.

## 1.2 Projenin Amacı ve Hedefleri

**Amaç:** Türkçe junior ve mid-level backend ilanları için bir ilana ait birden fazla Türkçe özgeçmişi TF-IDF ve çok dilli cümle gömme yöntemleriyle sıralayıp, mümkün olduğunda insan yargılı metin-ilgi ölçütleriyle karşılaştırmak.

Hedefler:

1. Türkçe backend ilanları ve CV’ler için veri kaynağı, alan, provenans, izin ve saklama koşullarını doğrulamak.
2. İşle ilgili alanları tanımlamak; doğrudan tanımlayıcıları, hassas özellikleri ve bunlara vekil olabilecek alanları model/etiket girdisinden çıkarmak.
3. Aynı redakte metin üzerinde TF-IDF + cosine ve çok dilli sentence embedding + cosine yöntemlerini tekrarlanabilir biçimde kurmak.
4. Zorunlu beceri kapsamını eşleşen metinsel kanıtla ayrı göstermek ve iki model skoruna karıştırmamak.
5. İzinli veri ve yeterli liste derinliği sağlanırsa bağımsız 0/1/2 etiketleriyle sıralama ölçütleri, anotatör uyumu ve hata türlerini raporlamak.
6. Yerel React arayüzü, FastAPI servisi ve SQLite kayıt katmanı içeren bir araştırma prototipi sunmak.
7. Gerçekleştirilen uygulama, test ve deneyleri planlanan çalışmalardan ayırarak öğretmenin istediği ayrı Gerçekleştirim Raporu’nda belgelemek.

Bu aşamada tamamlanmış yazılım, toplanmış araştırma verisi, anotasyon veya ölçülmüş model sonucu yoktur.

## 1.3 Projenin Kapsamı

Araştırma Türkçe CV’ler ile Türkçe **backend software developer** ilanlarını kapsar. Junior ve mid-level roller dahil; senior, lead ve yöneticilik pozisyonları dışarıda bırakılır. Her ilan birden fazla CV ile karşılaştırılır. Yöntemler TF-IDF + cosine ve çok dilli cümle gömmeleri + cosine’dir. Zorunlu teknik beceri kapsamı ayrıca açıklanır. Türkçe metin içindeki İngilizce teknik adlar (ör. Python, SQL, API) korunur; İngilizce CV’ler ve diller arası eşleştirme başlangıç kapsamı dışındadır.

Prototip yerelde çalışacak şekilde planlanır: React/TypeScript/Vite, Python/FastAPI ve SQLite/SQLAlchemy/Alembic. OCR, dış bulut/ücretli yapay zekâ hizmeti, işe alım veya otomatik eleme kararı, demografik çıkarım, kalibre edilmiş işe uygunluk olasılığı ve site kazıma bu çalışmanın kapsamı değildir. Kaynak özelinde izin/erişim koşulları sonradan açıkça incelenmeden kazıma yapılmayacaktır.

# 2. LİTERATÜR VE MEVCUT ÇALIŞMALAR

## 2.1 Konuyla İlgili Çalışmalar

Li ve arkadaşları (2020), klinik araştırma koordinatörü pozisyonları için 6.492 CV’yi ve 24.933 başvuru içinden 252 pozisyonu inceleyerek deneyim düzeyi sınıflandırması ve CV–ilan eşleştirmesi kurmuştur. Çalışma uzman anotasyonu, ilan bağlamı ve bölüm temelli belge temsilinin önemini gösterir. Verisi İngilizce ve sağlık alanına aittir; geliştirilen sınıflandırma sonuçları bu projenin Türkçe backend cosine sıralamasına genellenemez.

Kaya ve Bogers (2023), İskandinav bir iş portalındaki geçmiş CV–ilan eşleşme/uyuşmazlık kayıtlarıyla BERT tabanlı çift sınıflandırmasını Sentence-BERT ve başka yaklaşımlarla karşılaştırmış, çevrimdışı ve çevrimiçi değerlendirmeler bildirmiştir. Bu çalışma davranışsal eşleşme etiketleri olduğunda denetimli çift modellemesinin değerini gösterir. Ancak bu tür tarihsel portal verisi bu projede doğrulanmış değildir ve planlanan sabit embedding-cosine yönteminden farklıdır.

Vanetik ve Kogan (2023), yazılım geliştirici CV’leri ve iş ilanları üzerinde tam metin/özet, kelime ve karakter n-gramları, TF-IDF ve cümle gömmeleri gibi temsilleri değerlendirmiştir. İnsan ve otomatik değerlendirme içerir. Yazarların deneylerinde karakter n-gramları güçlü sonuçlar verirken TF-IDF bazı koşullarda daha zayıf kalmış ve belirli gösterimlerde L1 uzaklığı cosine’dan daha iyi sonuç vermiştir. Bu bulgu, bu projede seçilen iki cosine hattının Türkçe hedef veri üzerinde sınanması gerektiğini ve önceden başarı varsayılmaması gerektiğini gösterir. Çalışmanın dili/örneklemi ve veri kaynağı Türkiye bağlamına doğrudan aktarılmaz.

Çöltekin, Doğruöz ve Çetinoğlu’nun (2023) eleştirel taraması Türkçe korpus ve sözlüksel kaynakları, kapsama ve açıklarla birlikte değerlendirir. Genel Türkçe NLP kaynağının iş ilanı/CV alanına uygunluğu kendiliğinden sağlanmış değildir; proje alanı için kaynakların ayrıca doğrulanması gerekir.

Vaishampayan ve arkadaşları (2025), gerçek iş başvurularına ait 736 özgeçmişte GPT-4 ile insan değerlendirmelerini karşılaştırmış ve puanların insan yargısıyla birbirinin yerine kullanılamayacağını bildirmiştir. Çalışma insan puanlamasının da belirli bir değerlendirme yapısı ve sınırlamaları bulunduğunu hatırlatır. Verisi özel bir sektör ortağından alınmış, İngilizce ve farklı iş alanlarını kapsar; bu nedenle yeniden kullanılabilir Türkçe veri kaynağı değildir.

İltüzer ve arkadaşlarının (2026) arXiv ön baskısı, farklı iş kollarından 327 Türkçe ilan üzerinde 4.819 elle etiketlenmiş beceri parçası bildirmektedir. Bu çalışma beceri çıkarımının Türkçe’de ayrı bir problem olduğuna işaret eder; ancak CV–ilan sıralama yargıları sağlamaz. Ön baskı kaydının varlığı, veri kümesinin erişilebilir veya bu proje için lisanslı olduğunu göstermez; bu bilgiler bilinmemektedir. ESCO gibi çok dilli bir sınıflandırma olası bir beceri normalizasyon kaynağı olabilir; seçilen sürüm ve kullanım koşulları ayrıca doğrulanmalıdır (European Commission, n.d.).

## 2.2 Mevcut Yaklaşımların Değerlendirilmesi

Literatür tek bir en iyi yöntem göstermemektedir. TF-IDF ve n-gramlar sözcüksel örtüşmeyi açık biçimde yakalar; karakter n-gramları yazım ve ek farklarına dayanıklı olabilir, ancak ilan metnindeki genel kalıplar skoru şişirebilir. Cümle gömmeleri ifade biçimleri değişse bile semantik yakınlık yakalayabilir; bununla birlikte genel amaçlı modelin Türkçe backend metinlerinde geçerli sıralama üreteceği garanti değildir. Geçmiş başvuru davranışlarına dayalı modeller zengin etiket ister ve “daha önce eşleşti” etiketini gerçek yeterlilik olarak yorumlama riski taşır.

Bu nedenle çalışma, iki basit ve tekrarlanabilir cosine yaklaşımını aynı belge alanlarında karşılaştırır ve beceri kapsamını ayrı tutar. İnsan anotasyonları yeterlilik veya işe alım sonucu olarak değil, belirli ilan gereksinimlerine göre CV metninde belgelenmiş ilgi kanıtı olarak tanımlanır. Sonuçlar dil, meslek, örneklem, kaynak izinleri ve annotasyon öznelliğiyle sınırlı yorumlanacaktır.

# 3. ÖNERİLEN ÇÖZÜM VE YÖNTEM

## 3.1 Önerilen Yaklaşım

Önerilen süreç; (1) izinli kaynak ve kayıt provenansını doğrulama, (2) uygun ilanları ve gönüllü/izinli CV’leri seçme, (3) ilgili bölümleri çıkarıp redakte etme, (4) iki modelle her ilan için CV listesi sıralama, (5) ayrı beceri kanıtı sunma, (6) skorları görmeyen değerlendiricilerin CV–ilan çiftlerini puanlaması ve (7) koşulları karşılayan sıralama metriklerini raporlamadan oluşur.

### Veri kaynakları ve edinme koşulları

Ön araştırmada kullanım hakkı doğrulanmış bir Türkçe backend CV–ilan eşleştirme kümesi saptanmamıştır. Dolayısıyla aşağıdaki edinim yolları plan niteliğindedir; izin veya erişim sağlandığı anlamına gelmez.

| Kaynak / aday | Erişim ve gerekli koşul | Kullanılabilecek alanlar | Mevcut durum / alternatif |
|---|---|---|---|
| İşveren/veri sahibi ilanları — birincil yol | Araştırma, işleme, yerel saklama, alıntı/özetle raporlama ve silme kapsamını açıkça içeren yazılı izinle kontrollü aktarım | Başlık, kıdem, görev/sorumluluk, zorunlu ve tercih edilen beceriler, açık deneyim/eğitim koşulları; tarih ve kaynak provenansı | Başvuru yapılmadı, izin yok, hacim bilinmiyor. İzin alınamazsa yalnızca uygun hakları yazılı biçimde doğrulanmış sağlayıcı kaynağı değerlendirilir. |
| Gönüllü Türkçe CV’leri — birincil yol | Kurumun gerekli araştırma/gizlilik süreci belirlendikten sonra bilgilendirilmiş katılım, uygun hukuki temel, güvenli doğrudan aktarım ve silme planı | Takma kimlik; işle ilgili özet, teknik beceri, deneyim/proje kanıtı; ilanın şart koştuğu eğitim/sertifika | Katılımcı, izin ve hacim yok. Gönüllüler ilgili ilanlara gerçek başvuruda bulunmamış olabilir; etiketler yalnızca metinsel ilgi yargısı olur. Kaynak sağlanamazsa gerçek veri performans iddiası yapılmaz. |
| Techmap / JobDataFeeds | Ücretli veri akışı; araştırma, saklama, metin işleme, alıntı/raporlama ve üst kaynak hakları yazılı teyit edilirse | İlan sayfasında bildirilen başlık, açıklama, konum, tür, beceri ve benzeri alanlar; dil/rol filtresi ancak veri üzerinde doğrulanır | Aylık hacim kaynak sayfalarında farklı; Türkçe/backend/junior-mid kapsamı ve proje kullanım hakkı bilinmiyor. Uygun yazılı haklar olmadan kullanılamaz. |
| Headcount Labs Türkiye ilan akışı | API/sağlayıcı erişim koşulları incelenebilir; ancak tam ilan metni için işveren izni gerekir | Metadata (başlık, işveren, tarih, URL, konum vb.); tam metin yalnızca hak sahibi izin verirse | Sağlayıcı deposu tam açıklamaların işveren mülkiyetinde olduğunu ve kendi lisansına dahil olmadığını belirtiyor. Meta-veri ilan keşfine yardımcı olabilir; metinsiz sıralama girdisi olamaz. |
| Göçmeden’in türetilmiş açık verisi | Sağlayıcının CC BY 4.0 olarak belirttiği türetilmiş kayıtları atıfla kullanma koşulları ayrıca kontrol edilir | Türetilmiş kategori, etiket, kısa Türkçe özet/alıntı ve tarih gibi yayımlanan alanlar | Tam özgün ilan metni dahil değildir; remote odaklı kapsamdır. Tam metinli ana deneyin yerine geçmez; yalnızca daraltılmış araştırma sorusu ayrıca gerekçelendirilirse kullanılabilir. |
| İltüzer ve arkadaşlarının beceri kaynağı | Makale ve yazarlarla erişim/lisans koşulu ayrıca doğrulanır | Ulaşılabiliyorsa ilan beceri parçaları ve ESCO bağlantıları | 327 ilan/4.819 beceri parçası makalede raporlanır; dosya erişimi ve kullanım lisansı bilinmiyor. CV veya eşleşme etiketi sağlamaz. |
| Sentetik yazılım test fikstürleri | Tamamen kurmaca, gerçek CV/ilan kopyalamayan ve sentetik olarak işaretlenmiş metinler proje içinde hazırlanabilir | Yazılım sınır durumları için başlık, görev, sahte beceri/deneyim alanları | Yalnızca yazılım testleri ve demo; insan etiketli araştırma verisi veya model etkinliği kanıtı değildir. Sentetik test sonuçları ampirik model performansı olarak raporlanmaz. |

Bu raporda belirli bir veri sağlayıcısına izin başvurusu yapıldığı veya izin alındığı iddia edilmemektedir. İzinli işveren ilanları ve gönüllü CV’leri için önerilen küçük fizibilite pilotu 5 ilan ve 10 benzersiz CV’dir; her CV her ilana eşlenirse 50 çift oluşur. Bu bir planlama hedefidir, edinilmiş veri veya istatistiksel yeterlilik iddiası değildir. Beş ilandan genellenebilir performans sonucu çıkarılmayacaktır. Ayrıntılı aday kaynak kanıtları ve bilinmeyenler `docs/RESEARCH_FEASIBILITY.md` dosyasındadır.

### Kullanılacak veri alanları ve gizlilik

İlan için başlık ve kıdem uygunluğu, backend sorumlulukları, açık zorunlu/tercih edilen teknik beceriler ve açıkça belirtilmiş deneyim/eğitim koşulları tutulur. CV için rol-ilişkili özet, teknik beceriler, backend iş deneyimi ve proje katkıları; ayrıca yalnızca ilan gerektiriyorsa eğitim/sertifika bilgisi kullanılır. Tam isim, fotoğraf, e-posta, telefon, kişisel bağlantılar, tam adres, doğum tarihi/yaş, cinsiyet, uyruk, aile/sağlık verisi, referans, okul ve eski işveren itibarı/ismi, ücret ve ilgisiz hobiler özellik olarak kullanılmaz. İlan şirket adı ve tanıtım metni model metnine katılmaz.

CV metni kişisel veri olduğundan kaynak, amaç, bilgilendirme/uygun hukuki dayanak, erişim, saklama/silme, yayın ve kurum incelemesi gereklilikleri belirlenmeden CV toplanmaz veya işlenmez. İzinli dosyalar özel yerel alanda tutulur; Git’e, loglara, ekran görüntülerine veya harici AI hizmetlerine konulmaz. Anotasyon için doğrudan tanımlayıcılar kaldırılır, kayıtlar takma kimlikle gösterilir; redaksiyonun iş kanıtını silmediği kontrol edilir. Kamuya açık ilanların kazınması varsayılan yöntem değildir. Bu rapor hukuki görüş değildir; uygulama öncesi kurumsal veri koruma/etik gereklilikleri doğrulanacaktır.

## 3.2 Kullanılacak Yöntem ve Teknolojiler

### Ön işleme

İzinli dijital PDF/DOCX belgelerinin metni çıkarılır; taranmış, bozuk veya şifreli dosyalar sessizce boş metne çevrilmez, ayrıştırma hatası olarak kaydedilir. OCR bu kapsamın dışındadır. Bölüm başlıkları ve insan spot kontrolüyle uygun içerik alanları ayrılır. Tekrarlı başlık/altbilgi ve ilgisiz boilerplate denetlenebilir kurallarla çıkarılır. Kimlik ve dışlanan alanlar anotasyon ve modellemeden önce redakte edilir. Satır sonu/boşluklar normalleştirilir; Unicode NFC uygulanır; Türkçe I/İ dönüşümü gözetilir. C++, C#, .NET, Node.js ve CI/CD gibi noktalama içeren teknik adlar korunur. Çeviri, LLM özeti, kök bulma veya teknik adları bozan genel noktalama temizliği yapılmaz. Türkçe olmayan CV’ler ana deneyden dışlanır; Türkçe metin içindeki İngilizce teknik terimler korunur.

### Yöntem A: TF-IDF + cosine

Gözden geçirilmiş beceri sözlüğündeki tam eşanlamlılar, en uzun eşleşme önceliğiyle kanonik belirteçlere çevrilir. Kelime kanalı Unicode uyumlu tokenlarla unigram/bigram; karakter kanalı `char_wb` 3–5 gram kullanır. Her iki TF-IDF kanalı kendi içinde L2 normalize edilir, eşit ağırlıkla birleştirilir ve ortak vektör tekrar L2 normalize edilir. IDF/vocabulary sadece eğitim/referans bölmesinde öğrenilir; değerlendirme metinlerine yeniden fit edilmez.

İlan (j) ve CV (i) için birleştirilmiş vektörler (x_j,x_i) olmak üzere:

\[
S_{TFIDF}(j,i)=\frac{x_j\cdot x_i}{\|x_j\|_2\,\|x_i\|_2}.
\]

Sıfır vektör veya kullanılabilir özellik yoksa skor “hesaplanamadı” olarak gösterilir. Kesin vectorizer ayarları, tokenizer/lexicon sürümü ve modelleme kayıtları `docs/NLP_AND_SCORING.md`’de belirlenmiştir.

### Yöntem B: Çok dilli sentence embedding + cosine

Başlangıç adayı `sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2`’dir. Model kartı çok dilli kullanım, 384 boyut ve 128 token azami uzunluk bilgisi sunar; bunlar Türkçe backend sıralama başarısını kanıtlamaz. Tam model revizyonu, lisans, bağımlılık sürümleri, CPU/yerel kaynak ihtiyacı ve kullanım koşulları indirme/uygulamadan önce doğrulanır.

Uzun metinler bölüm sınırlarını koruyarak model token sayısına göre örtüşmesiz kısa parçalara ayrılır. Her parça vektörü (z_c) L2 normalize edilir; (n_c) o parçadaki içerik token sayısı olsun. Belge gömme vektörü:

\[
e(D)=\operatorname{normalize}\left(\frac{\sum_c n_c z_c}{\sum_c n_c}\right).
\]

İlan ve CV skoru:

\[
S_{emb}(j,i)=\frac{e(j)\cdot e(i)}{\|e(j)\|_2\,\|e(i)\|_2}.
\]

Kullanılabilir parçası olmayan belge için skor hesaplanamaz. Parça uzunluğu ve sürüm gibi kesin kararlar yöntem belgesinde sabitlenir. Başlangıçta fine-tuning planlanmamaktadır.

### Skorların anlamı ve beceri kapsamı

İki (S) değeri ayrı, ham cosine metin-benzerliği sinyalleridir; aday yeterliliği, olasılık, yüzde, kalibre edilmiş işe uygunluk veya işe alım kararı değildir. İki yöntemin skorları birbirine eklenmez.

İlanın açık zorunlu beceri kümesi (R_j), CV’de kanonik beceri sözlüğüyle olumlu metinsel kanıtı bulunan beceri kümesi (C_i) olmak üzere:

\[
Coverage_{req}(j,i)=\frac{|R_j\cap C_i|}{|R_j|},\quad |R_j|>0.
\]

Bu oran yalnızca CV metninde zorunlu becerilerin bulunma oranıdır. Payda yoksa N/A gösterilir. Beceri yokluğu “CV metninde tespit edilmedi” şeklinde raporlanır; kişinin beceriden yoksun olduğuna hükmedilmez. Olumsuz/belirsiz bağlamlar olumlu beceri sayılmaz; eşleşme kanıtı gösterilir. Tercih edilen beceri kapsamı gerekirse ayrı raporlanır. Beceri kapsamı cosine skorlarına karıştırılmaz.

## 3.3 Sistem Tasarımı veya Çalışma Süreci

Planlanan yerel veri akışı:

```text
React + TypeScript (localhost:5173)
  → FastAPI (localhost:8000)
  → izinli dosya/metin çıkarımı ve güvenli validasyon
  → alan seçimi, kimlik redaksiyonu ve normalize etme
  → beceri eşleme kanıtı + TF-IDF skoru + sentence-embedding skoru
  → seçilen yöntemle ilan başına CV sıralaması
  → SQLite metadata/çalıştırma/sonuç özeti ve özel yerel dosya deposu
```

React istemcisi SQLite’a veya dosya yoluna doğrudan erişmez. FastAPI uçları ince tutulur; metin çıkarımı, beceri tespiti ve model çalıştırma servis katmanında ayrılır. SQLite yalnızca gereken metadata, provenans/izin kaydı, analiz konfigürasyonu, etiket bağlantısı ve skor özetlerini tutar. Ham CV dosyaları gerekiyorsa rastgele adlandırılmış, özel ve Git dışı depoda saklanır. Kullanıcı iki modelden biriyle sıralama seçebilir; her iki ham skor ve yöntem açıklaması görünür. Dosya ayrıştırma hatası, bulunmayan beceri ve hesaplanamayan skor açıkça belirtilir. Silme, ilişkili dosya ve kayıtları da kapsar. Mimari planlanmıştır; uygulama henüz yoktur.

# 4. PROJE PLANI

## 4.1 Yapılacak Çalışmalar

Uygulama ve araştırma çalışmaları aşağıdaki sırayla planlanır. Gerekli kişisel veri/ilan metni izinleri olmadan gerçek veriye özgü adımlar yürütülmez; yazılım bileşenleri açıkça sentetik test fikstürleriyle geliştirilebilir.

1. **Kaynak ve provenans doğrulama:** kaynak kullanım koşulu/lisansı, upstream haklar, dil, alan şeması, hacim, rol seviyesi, yinelenen kayıt ve etiket kaynağını doğrula. Verileri edinmeden önce veri sözlüğü/provenans manifesti hazırla.
2. **Güvenli veri hattı:** yalnızca izinli veriyi kabul et; CV ve ilan verisini ayır; dosya türü/limitini doğrula; PDF/DOCX metin çıkarım hatalarını kaydet; uygun alanları çıkarıp kimlikleri redakte et; Türkçe normalizasyonu ve veri minimizasyonunu uygula; testlerde sentetik fikstür kullan.
3. **TF-IDF baseline:** beceri alias normalizasyonu, kelime unigram/bigram ve karakter 3–5 gram kanallarını kur; fit/transform sınırını ve konfigürasyonu sürümle; cosine skorunu üret.
4. **Sentence embedding hattı:** model ve lisans revizyonunu doğrula; bağımlılıkları ve yerel çalıştırma kaynaklarını sabitle; uzun metni bölüm korumalı parçalara ayır; parça vektörlerinden belge embedding’i ve cosine skorunu üret.
5. **Beceri çıkarımı:** sürümlü Türkçe backend beceri sözlüğü ve alias seti oluştur; sınır/olumsuzluk/belirsizlik durumlarını test et; ilan zorunlu/tercih edilen becerilerini ayır; CV’de bulunan ve metinde bulunmayan durumları kanıtla birlikte göster.
6. **Sıralama ve kayıt:** her bir ilan için birden fazla CV’yi seçilen model skoruyla sırala; iki skor, sıra, sürüm, parametre, veri snapshot/provenansı ve kanıtları kaydet; N/A/hata durumlarını tanımla.
7. **Değerlendirme kümesi:** uygun izinler/kurumsal süreç sağlanırsa ilan bazında çoklu CV listesi hazırla; sabit aday havuzlarını iki değerlendiriciye model çıktısı olmadan ver; 0/1/2 yargı, kanıt pasajı, uyuşmazlık çözümü ve leakage-safe ayrımı sürümle. Sentetik etiketleri test metriği yapma.
8. **Metrikler ve hata incelemesi:** geçerli ilanlar için her modelin NDCG@5 değerini ve farkını hesapla; koşulları sağlanıyorsa Kendall tau-b, Precision@5/Recall@5 ve anotatör uyumunu raporla; geçersiz/eksik metriği gerekçelendir; hata örneklerini kimliksizleştir.
9. **FastAPI backend:** şemalar, validasyon, yerel health endpoint, job/candidate CRUD, yükleme ve silme, matching-run ve evaluation özet endpoint’lerini geliştir; güvenli hata/log politikası uygula.
10. **SQLite veri katmanı:** SQLAlchemy modelleri ve Alembic migration’ları; iş, aday, eşleşme, model çalıştırma, beceri kanıtı ve araştırma etiketi ilişkileri; silme ve dosya saklama davranışı.
11. **React frontend:** iş listesi/oluşturma, tek ilan ayrıntısı, çoklu CV yükleme, parse durumları, yöntem seçilebilir sıralama, beceri kanıtı, hata/boş/loading durumları, CSV dışa aktarma ve yerel silme akışı.
12. **Otomatik testler ve kalite:** backend birim/API/DB/gizlilik testleri, frontend bileşen/akış testleri, lint/typecheck/build ve sentetik uçtan uca fikstür. Gerçek veriyle test ancak izin varsa; hiçbir test CV metnini loglamamalı. Başarısız testler sessiz geçilmemeli.
13. **Raporlama ve teslim:** Analiz Raporu planlanan yöntemi ve gerçek araştırma durumunu anlatır. Gerçekleştirim Raporu yalnızca uygulanmış özellik, test ve ölçümleri; veri provenansı, etik/gizlilik, sınırlılıklar, APA 7 ve gerekli AI kullanım açıklamasını bildirir. DOCX/PDF biçiminde öğretmen yönergesi uygulanır.

Geliştirme kontrol listesi ayrıca `TASKS.md` içinde sıralanmıştır. Bu Analiz Raporu uygulama kodu veya kullanıcı verisi toplamaya başlamaz.

## 4.2 Beklenen Çıktılar

- Kaynak izinleri ve provenansı belgelenmiş veri sözlüğü; izinli kaynak elde edilemezse engel ve alternatif kaydı.
- Türkçe junior/mid backend ilan ve CV metinleri için minimizasyon, redaksiyon ve ön işleme tanımı.
- Tek ilanı çoklu Türkçe CV ile karşılaştıran yerel React/FastAPI/SQLite prototipi.
- Tekrarlanabilir TF-IDF ve çok dilli embedding skorları; ayrı, kanıtlı zorunlu beceri kapsamı.
- İzinli ve bağımsız etiketli veri yeterliyse NDCG@5 dahil karşılaştırma ve hata analizi. Yeterli veri yoksa ölçüm yapılmadığının açıkça raporlanması.
- Uygun sentetik yazılım test fikstürleri; bu fikstürlerin model etkinliği kanıtı sayılmaması.
- Öğretmen biçimine uygun Analiz Raporu ve yalnızca fiilen gerçekleştirilen çalışmaları içeren Gerçekleştirim Raporu.

# 5. SONUÇ

Bu Analiz Raporu, tek meslek ve tek dil kapsamındaki çoklu özgeçmiş sıralama problemini, iki karşılaştırılabilir cosine yaklaşımını, ayrı beceri kanıtını, izinli veri edinme yolunu ve koşullu insan değerlendirmesini tanımlar. Akademik çalışmalar iş-özel anotasyonun ve sıralama değerlendirmesinin önemini gösterse de farklı ülke, dil, meslek ve veri yapılarından gelen sonuçlar Türkçe backend performansının kanıtı değildir. Şu anda bu araştırma için onaylı bir Türkçe eşleştirme veri kümesi, işveren kullanım izni, gönüllü CV kaynağı, anotasyon veya model sonucu mevcut değildir. İlk uygulama adımı yazılımı sentetik test fikstürleriyle kurabilir; gerçek veriye geçiş ancak kaynak hakları, gizlilik temeli, kurumsal koşullar ve provenans doğrulandıktan sonra yapılabilir. Sentetik testler yazılım davranışını sınar, araştırma başarısını göstermez. Sonuç olarak projenin ampirik karşılaştırma çıktısı veri erişimine ve geçerli, bağımsız ilan gruplarına bağlıdır.

# 6. KAYNAKÇA

Çöltekin, Ç., Doğruöz, A. S., & Çetinoğlu, Ö. (2023). Resources for Turkish natural language processing: A critical survey. *Language Resources and Evaluation, 57*(1), 449–488. https://doi.org/10.1007/s10579-022-09605-4

European Commission. (n.d.). *ESCO classification*. https://esco.ec.europa.eu/en/use-esco

İltüzer, E. A., Özlü, Ö. A., Farajijobehdar, V., & Eryiğit, G. (2026). *Leveraging LLMs for Turkish skill extraction* [Preprint]. arXiv. https://doi.org/10.48550/arXiv.2601.22885

Kaya, M., & Bogers, T. (2023). An exploration of sentence-pair classification for algorithmic recruiting. In *Proceedings of the 17th ACM Conference on Recommender Systems (RecSys 2023)* (pp. 1175–1179). Association for Computing Machinery. https://doi.org/10.1145/3604915.3610657

Li, C., Fisher, E., Thomas, R., Pittard, S., Hertzberg, V., & Choi, J. D. (2020). Competence-level prediction and resume & job description matching using context-aware transformer models. In *Proceedings of the 2020 Conference on Empirical Methods in Natural Language Processing* (pp. 8456–8466). Association for Computational Linguistics. https://doi.org/10.18653/v1/2020.emnlp-main.679

Vaishampayan, S., Leary, H., Alebachew, Y. B., Hickman, L., Stevenor, B., Beck, W., & Brown, C. (2025). Human and LLM-based resume matching: An observational study. In *Findings of the Association for Computational Linguistics: NAACL 2025* (pp. 4823–4838). Association for Computational Linguistics. https://doi.org/10.18653/v1/2025.findings-naacl.270

Vanetik, N., & Kogan, G. (2023). Job vacancy ranking with sentence embeddings, keywords, and named entities. *Information, 14*(8), Article 468. https://doi.org/10.3390/info14080468

## Ek: AI kullanım beyanı için not

Öğretmen yönergesi AI aracı kullanılmışsa araç ve kullanım amacının rapor sonunda veya ekte belirtilmesini ister. Teslim öncesi öğrenci, kullandığı araçları ve gerçek kullanım amacını kendi çalışmasına uygun biçimde açıklamalı; kaynak, yöntem, yorum ve rapor içeriğinin doğruluğunu kendisi kontrol etmelidir. Bu not tamamlanmış deney/öğrenci katkısı iddiası değildir.
