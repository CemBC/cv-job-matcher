# Kısa akademik proje önerisi

**Durum:** Öğretmen incelemesi için taslaktır. Akademik proje onayı, veri izni, veri toplama, model çalıştırma veya ölçülmüş sonuç iddia etmez. Bu öneri, daha sonra hazırlanacak Analiz Raporu ve çalışma tamamlanınca yazılacak Gerçekleştirim Raporu yerine geçmez. Özgün öğretmen rapor yönergesi `docs/reference/` altında tutulur ve yetkili kaynaktır.

## Proje başlığı

**Türkçe Özgeçmişler ile İş İlanlarının Doğal Dil İşleme Yöntemleriyle Eşleştirilmesi ve Uygunluk Sıralaması**

## Problem, amaç ve hedefler

Türkçe backend yazılım geliştirici ilanları için birden fazla özgeçmişteki işle ilgili metinsel kanıtı tutarlı biçimde karşılaştırmak güçtür. Anahtar sözcük yöntemleri ifade farklılıklarını ve Türkçe ekleri kaçırabilir; anlamsal benzerlik de tek başına mesleki yeterlilik veya işe alım başarısı göstermez. Türkçe backend rolleri için çoklu CV sıralamasını bağımsız insan yargılarıyla değerlendirecek izinli ve yeniden üretilebilir veri bulmak da temel bir araştırma kısıtıdır.

**Birincil amaç:** Türkçe junior ve mid-level backend ilanları için bir ilana ait birden fazla Türkçe özgeçmişi TF-IDF ve çok dilli cümle gömmeleriyle sıralayarak yöntemleri insan yargılı metin-ilgi ölçütleriyle karşılaştırmak.

Bu karşılaştırma yalnızca ilan ve CV metinlerini araştırmada kullanma hakları ile gerekli kurumsal süreçler doğrulanırsa yürütülecektir. NDCG@5, her ilan için en az beş aday, sabit ve tam yargılanmış aday havuzu ve pozitif ideal kazanç varsa birincil metrik olacaktır; bu koşullar sağlanmazsa çalışma fizibilite/betimleyici pilotla sınırlı raporlanır.

Somut hedefler: (1) olası Türkçe ilan ve CV kaynaklarının erişim, kullanım ve raporlama koşullarını doğrulamak; (2) işle ilgili alanları belirleyip doğrudan tanımlayıcıları ve hassas/proxy özellikleri çıkarmak; (3) aynı temizlenmiş metinde iki sıralama yöntemini tekrarlanabilir biçimde tanımlamak; (4) kanıt gösteren zorunlu beceri kapsamını model skorlarından ayrı sunmak; (5) veri yeterliyse bağımsız insan etiketleriyle geçerli sıralama ölçütleri ve hata incelemesi raporlamak; (6) yerel React, FastAPI ve SQLite prototipi geliştirmek.

## İlgili literatür

Li ve arkadaşları (2020), klinik araştırma koordinatörü ilanları için uzman anotasyonlu CV’lerle bağlama duyarlı sınıflandırma ve CV–ilan eşleştirmesini inceler; işe özgü ölçütlerin önemini gösterir, ancak İngilizce sağlık alanı sonuçları bu projeye doğrudan taşınamaz. Kaya ve Bogers (2023), İskandinav iş portalının geçmiş eşleşme verileriyle BERT çift sınıflandırmasını Sentence-BERT’e karşılaştırır; davranışsal etiketlerin yararını gösterse de bu proje için benzer özel etiketli veri doğrulanmamıştır. Vanetik ve Kogan (2023), yazılım geliştirici CV’leri ve ilanlarında TF-IDF, n-gram ve cümle gösterimlerini insan/otomatik sıralama değerlendirmesiyle karşılaştırır; karakter n-gramlarının katkısı ve cosine dışı ölçümlerin bazı koşullarda üstünlüğü, bu projedeki cosine seçiminin ayrıca sınanması gerektiğini gösterir. Çöltekin ve arkadaşları (2023), Türkçe NLP kaynakları ve açıklarını tarar; genel dil kaynaklarının alan-özel uygunluğunu garanti etmediğini ortaya koyar. İltüzer ve arkadaşlarının (2026) arXiv ön baskısı Türkçe ilanlarda beceri çıkarımını inceler; CV–ilan eşleştirme yargısı sağlamaz ve veri kullanım koşulları bu çalışma için doğrulanmış değildir. Bu literatür yöntem tasarımına dayanak sağlar, Türkçe backend sıralama başarısını önceden kanıtlamaz (Çöltekin et al., 2023; İltüzer et al., 2026; Kaya & Bogers, 2023; Li et al., 2020; Vanetik & Kogan, 2023).

## Veri kaynakları ve edinme planı

Ön araştırmada bu çalışma için kullanımı doğrulanmış Türkçe CV–ilan veri kümesi veya bağımsız çoklu-CV/ilan yargıları saptanmamıştır. Aşağıdaki pilot hacimleri yalnızca planlama hedefidir; erişilmiş kayıt, izin veya katılımcı taahhüdü değildir.

| Kaynak kategorisi | Önerilen edinme yöntemi | Toplanacak asgari alanlar | Mevcut erişim/izin durumu | Pilot tasarım hedefi | Fallback |
|---|---|---|---|---:|---|
| Türkçe backend iş ilanları | İşveren/veri sahibinden, araştırma, saklama ve raporda kullanım kapsamını belirten yazılı izin ve kontrollü aktarım istemek | İlan kimliği/tarihi; başlık ve kıdem; görevler; zorunlu/tercih edilen beceriler; açık deneyim/eğitim koşulları; izin/provenans kaydı | İzin alınmadı; hedef kapsamda ilan adedi bilinmiyor. Kamuya açık olması yeniden kullanım izni sayılmaz; kazıma yapılmayacak. | 5 junior/mid ilan | Yazılı kullanım koşulları aynı hakları açıkça kapsarsa ticari sağlayıcıyı değerlendirmek; aksi hâlde ilan toplama ve gerçek veri ölçümünü durdurup kapsam için öğretmene danışmak. |
| Türkçe backend CV’leri | Kurumsal/etik ve gizlilik süreci doğrulandıktan sonra gönüllülerden doğrudan bilgilendirilmiş katılım ve güvenli aktarım | Takma kimlik; işle ilgili özet, beceriler, backend deneyim/proje kanıtı; yalnızca ilanın şart koştuğu eğitim/sertifika; izin ve silme durumu | Kaynak, katılımcı veya izin yok; örneklem büyüklüğü bilinmiyor. | 10 benzersiz CV; her CV her ilana eşlenirse 50 çift | İzinli, gönüllü CV edinimi mümkün değilse gerçek CV eşleştirme performansı iddiasını bırakıp yalnızca açıkça sentetik, demo amaçlı örnekler önermek; sentetik etiketleri metriklere katmamak. |
| İlan metni için ticari sağlayıcı (koşullu) | Sağlayıcıyla yazılı görüşme; tam metin işleme, saklama, analiz ve rapor/alıntı haklarını teyit etmek | Yukarıdaki ilan alanları; ayrıca sağlayıcı ve upstream kaynak provenansı | Techmap/JobDataFeeds ticari; proje hakları ve Türkçe/backend/junior-mid payı doğrulanmış değil. Headcount Labs, tam açıklamaların işveren mülkiyetinde olduğunu ve kendi lisansında olmadığını bildiriyor. Göçmeden’in lisanslı türetilmiş verisi tam ilan metni içermiyor. | Birincil pilot hedefinin yerine geçmez; uygunluk ve haklar teyit edilirse aynı 5 ilan hedefi | İzin yoksa sağlayıcı metnini kullanmamak; işveren izni yoluna dönmek veya gerçek veri çalışmasını durdurmak. |

Önerilen pilot tasarımı 5 iş ilanı ve 10 benzersiz CV’dir. Her CV her ilanla değerlendirilirse tam matris 50 CV–ilan çifti üretir. Bu yalnızca planlama hedefidir; veri toplanmamış ve izin alınmamıştır. Beş ilan küçük ve sınırlı bir örneklemdir; genellenebilir performans iddiasını desteklemez. NDCG@5’in geçerliliği ayrıca her ilanın sabit aday listesinin tamamının yargılanmasına ve pozitif ideal kazanca bağlıdır. Haklar, CV’lerin bilgilendirme/işleme süreci ve ilgili kurum gereklilikleri netleşmeden veri toplanmayacaktır. Gerçek ilan ve gönüllü CV’leri eşleşmediğinde insan etiketleri yalnızca metinsel ilgi yargısıdır; işe alım sonucu ya da aday yeterliliği değildir. Sentetik, tamamen kurgusal metinler demo ve kenar durumlarla sınırlıdır; performans değerlendirmesine girmez.

İlanlardan başlık/kıdem, backend görevleri, açık zorunlu ve tercih edilen beceriler ile açık deneyim/eğitim koşulları; CV’lerden işle ilgili özet, beceriler, backend deneyim ve proje kanıtları ile yalnızca ilanın istediği eğitim/sertifika bilgisi alınır. İsim, iletişim, fotoğraf, yaş, cinsiyet, uyruk, aile/sağlık bilgileri, tam adres, okul/işveren itibarı, ücret ve ilgisiz içerik kullanılmaz. Belgeler takma kimliklerle, erişim kısıtı ve saklama/silme süreciyle korunur; yalnızca izin verilen kimliksizleştirilmiş bulgular raporlanır.

## Yöntem ve skorlar

İzinli dijital PDF/DOCX metinlerinden ilgili bölümler çıkarılır ve insan kontrolüyle tanımlayıcılar redakte edilir. Satır sonları/boşluklar normalize edilir; Unicode NFC ve Türkçe I/İ duyarlı küçük harf dönüşümü uygulanır; C++, C#, .NET, Node.js, CI/CD ve SQL gibi teknik yazımlar korunur. OCR ve LLM ile çeviri/özetleme kapsam dışıdır. Her iki model aynı redakte alanları kullanır.

1. **TF-IDF + cosine:** Kanonik beceri eşlemesi sonrasında kelime unigram/bigram ve karakter 3–5 gram TF-IDF kanalları eşit ağırlıkla birleştirilir. Eğitim/referans bölümünde öğrenilmiş ilan ve CV vektörleri (x_j,x_i) için (S_{TFIDF}(j,i)=\frac{x_j\cdot x_i}{\|x_j\|_2\|x_i\|_2}).
2. **Çok dilli cümle gömmeleri + cosine:** Aday model `sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2`’dir; tam model revizyonu ve bağımlılıklar kullanım öncesi doğrulanacaktır. Uzun metinler bölüm sınırlarını koruyan kısa parçalara çevrilir, parça vektörlerinin uzunluk ağırlıklı ortalamasından belge vektörü (e(D)) elde edilir; (S_{emb}(j,i)=\frac{e(j)\cdot e(i)}{\|e(j)\|_2\|e(i)\|_2}).
3. **Bağımsız zorunlu beceri kapsamı:** İlandaki açık zorunlu beceriler (R_j), CV’de sözlükle ve metinsel kanıtla bulunan beceriler (C_i) ise (Coverage_{req}(j,i)=\frac{|R_j\cap C_i|}{|R_j|}) ((|R_j|>0)). Bu değer yalnızca CV metninde beceri anılma oranıdır; yeterlilik yüzdesi değildir. Eşleşen kanıt gösterilir; “CV metninde tespit edilmedi” ifadesi kişinin o beceriye sahip olmadığını göstermez. Kapsam oranı cosine skorlarına eklenmez.

Cosine skorları ayrı ham metin-benzerliği sinyalleridir; yüzde, olasılık, kalibre edilmiş iş-uygunluğu veya işe alım tavsiyesi olarak sunulmaz. Uzun belge parçalama, tokenizer, sözlük ve model sürümü ayrıntıları [`NLP_AND_SCORING.md`](NLP_AND_SCORING.md)’de tutulur.

## Yerel mimari ve veri akışı

Planlanan yerel mimari: React/TypeScript arayüzü (localhost:5173) izinli ilanı ve birden fazla CV’yi seçer; FastAPI (localhost:8000) metin çıkarımı, minimizasyon/redaksiyon, beceri kanıtı ve iki bağımsız sıralama yöntemini yürütür; SQLite kayıt metaverisini, izin/provenansı, yöntem ayarlarını ve sonuç özetlerini saklar. Gerekli ham CV dosyaları Git dışında erişimi kısıtlı yerel depoda tutulur. Tarayıcı veritabanına doğrudan erişmez. Bu tasarım henüz uygulanmamıştır. Gerçek kişisel CV veya kısıtlı ilan metni, kaynak hakları, gizlilik ve kurum gereklilikleri doğrulanana kadar işlenmez.

## Değerlendirme ve başarı koşulları

İki değerlendirici her anonim CV–ilan çiftini model skorlarını görmeden bağımsız puanlar: **0** ilgili metin kanıtı yok/çok sınırlı; **1** kısmi veya belirsiz kanıt; **2** temel görev ve zorunlu gereksinimleri destekleyen açık, özgül kanıt. Her puan ilan ölçütü ve CV pasajı/kanıt yokluğu ile gerekçelendirilir; ilk puanlar saklanır, çözülemeyen anlaşmazlık üçüncü değerlendiriciye gider. Etiket, metinsel ilgi yargısıdır; gerçek yeterlilik veya işe alım ground truth’u değildir.

Koşullu birincil metrik **NDCG@5**’tir. (rel_k\in\{0,1,2\}), gain (2^{rel_k}-1) ve discount (\log_2(k+1)) ile (DCG@5=\sum_{k=1}^{\min(5,n)}\frac{2^{rel_k}-1}{\log_2(k+1)}), (NDCG@5=DCG@5/IDCG@5). Her ilanın sabit aday havuzu tamamen yargılanmış, en az beş CV içermiş ve (IDCG@5>0) olmalıdır; aksi durumda NDCG@5 o ilan için hesaplanmaz. Koşullar sağlandığında her iki modelin NDCG@5 değerleri ayrı ayrı ve aralarındaki fark raporlanır; beş ilan üzerinden genellenebilir performans iddiası yapılmaz. İkincil Kendall tau-b ve önceden tanımlanmış eşikli Precision@5/Recall@5 yalnızca kendi geçerlilik koşulları sağlandığında raporlanır. Başarı, performans eşiği uydurmak yerine aynı izinli test listelerinde iki yöntemin tekrarlanabilir biçimde karşılaştırılması ve geçerli metriklerin/eksiklerin şeffafça raporlanmasıdır. Yeterli bağımsız ilan bulunmazsa sonuç betimleyici fizibilite pilotu olarak kalır. Anotasyon ayrıntıları ve metrik geçerlilik kuralları [`EVALUATION.md`](EVALUATION.md)’dedir.

## Beklenen çıktılar ve sınırlılıklar

Beklenenler: izin/provenans kaydı ve veri sözlüğü; gizlilik ve beceri kanıtı kuralları; iki yöntemi içeren localhost prototipi; veri yeterliyse karşılaştırmalı sıralama ölçümleri ve hata analizi; öğretmen yönergesine uygun Analiz Raporu ve yalnızca fiilen yapılanları anlatan Gerçekleştirim Raporu.

Veri erişimi, gönüllü örneklemin öz-seçimi, küçük ilan sayısı, anotatör yargısının öznelliği ve metin ilgisinin gerçek iş performansını temsil etmemesi başlıca sınırlılıklardır. Kimlikleri çıkarmak yanlılığın ortadan kalktığını kanıtlamaz; demografik veri toplanmadığından alt grup adaleti ölçülemez. İlan/CV izinleri, kurum prosedürü, nihai veri hacmi ve modelin sabit revizyonu bekleyen kararlardır.

## Kaynakça (APA 7)

Çöltekin, Ç., Doğruöz, A. S., & Çetinoğlu, Ö. (2023). Resources for Turkish natural language processing: A critical survey. *Language Resources and Evaluation, 57*(1), 449–488. https://doi.org/10.1007/s10579-022-09605-4

İltüzer, E. A., Özlü, Ö. A., Farajijobehdar, V., & Eryiğit, G. (2026). *Leveraging LLMs for Turkish skill extraction* [Preprint]. arXiv. https://doi.org/10.48550/arXiv.2601.22885

Kaya, M., & Bogers, T. (2023). An exploration of sentence-pair classification for algorithmic recruiting. In *Proceedings of the 17th ACM Conference on Recommender Systems (RecSys 2023)* (pp. 1175–1179). Association for Computing Machinery. https://doi.org/10.1145/3604915.3610657

Li, C., Fisher, E., Thomas, R., Pittard, S., Hertzberg, V., & Choi, J. D. (2020). Competence-level prediction and resume & job description matching using context-aware transformer models. In *Proceedings of the 2020 Conference on Empirical Methods in Natural Language Processing* (pp. 8456–8466). Association for Computational Linguistics. https://doi.org/10.18653/v1/2020.emnlp-main.679

Vanetik, N., & Kogan, G. (2023). Job vacancy ranking with sentence embeddings, keywords, and named entities. *Information, 14*(8), Article 468. https://doi.org/10.3390/info14080468

**Kaynak doğrulama ve durum notu:** İltüzer ve arkadaşlarının (2026) kaydı arXiv ön baskısıdır; hakemli yayın durumu veya veri kümesinin bu proje için erişim/kullanım izni doğrulanmamıştır. Yayıncı/akademik kayıtlar Çöltekin ve arkadaşları (2023), Kaya ve Bogers (2023), Li ve arkadaşları (2020) ve Vanetik ve Kogan (2023) künyelerini doğrular. İltüzer ve arkadaşları (2026) kaydı arXiv ön baskısı olarak doğrulanmıştır; hakemli yayın veya veri lisansı/erişimi olduğu varsayılmamıştır. Özgün rapor biçim ve bölüm gereklilikleri `docs/reference/` altındaki öğretmen DOCX’ine göre uygulanacaktır.
