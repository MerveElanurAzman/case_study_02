Enerji Perakende Veri Analizi — Case Study 2

Ahmet Çalık Vakfı İleri Veri Analitiği Eğitimi kapsamında hazırlanan, Amasya ilinin Hamamözü, Gümüşhacıköy ve Göynücek ilçelerine ait elektrik tüketim ve tahsilat verilerinin analizi.

Veri Seti

data/elektrik_veri_hashed.xlsx dosyası 5 sayfa içerir:

Sayfa	İçerik	Kayıt Sayısı
Tahsilat	Müşteri ödeme kayıtları (nakit/banka/mahsuben/kredi kartı)	636,993
Tahsilat 1	Aylık faturalar ve ödeme zamanlaması (16 zaman aralığı sütunu)	917,632
Tahakkuk	Hamamözü aylık elektrik tüketimi (kWh)	124,818
Tahakkuk 1	Gümüşhacıköy aylık elektrik tüketimi (kWh)	765,657
Tahakkuk 2	Göynücek aylık elektrik tüketimi (kWh)	295,223

Dönem: Ocak 2023 - günümüz. Not: Tahsilat ve Tahsilat 1 sayfaları Taşova ilçesini de içerir, ancak bu ilçeye ait tüketim (Tahakkuk) verisi veri setinde bulunmamaktadır — analizler bu kapsam farkı göz önünde bulundurularak yapılmıştır.

notebook_01 — Veri Keşfi ve Tanımlayıcı İstatistik: 5 sayfanın yapısal incelemesi (.info()/.describe()/.head()), ilçe bazlı benzersiz müşteri sayıları, pd.concat ile Tahakkuk sayfalarının birleştirilmesi ve doğrulanması, kwh sütununda eksik/negatif/aşırı uç değer tespiti, hesap sınıfına göre tanımlayıcı istatistik tablosu.

notebook_02 — Veri Görselleştirme: İlçe bazlı hesap sınıfı dağılımları (subplot), aylık ortalama tüketimin mevsimsel trend ve mevsim bazlı bar grafikleri, müşteri sayısı pasta grafiği, Tahsilat sayfası İlçe/Şube dağılımları, ödeme zamanlaması (zamanında/geç/kısmi) oranlarının pasta grafikleri, kwh dağılımının histogram ve box plot ile görselleştirilmesi.

notebook_03 — Veri Hikayesi Anlatımı: Açık uçlu analiz — ilçe karşılaştırma analizi (grouped bar + heatmap ile desteklenmiş), müşteri segmentasyonu ve (bonus) tahsilat performans analizi; her bölümde problem tanımı → hipotez → analiz → bulgu → iş önerisi akışı izlenmiştir.

Öne Çıkan Bulgular
Hesap sınıfı kompozisyonu (Mesken oranı ~%86-89) üç ilçede de neredeyse aynı; ilçeler arası tüketim farkının kaynağı müşteri tipi karışımı değil, mevsimsel davranış farkı. Heatmap'e göre Gümüşhacıköy hem ticari (%8.3) hem tarımsal (%1.8) faaliyette diğer ilçelerden bir miktar daha yüksek pay alıyor, Hamamözü en mesken ağırlıklı ilçe.
Hamamözü, diğer iki ilçenin aksine yaz aylarında belirgin bir tüketim artışı göstermiyor (muhtemelen coğrafi/iklimsel bir özellik).
kwh sütununda eksik değer yok; 151 negatif değer tespit edildi ve örüntü incelendiğinde bunların gerçek negatif tüketim değil, geriye dönük fatura düzeltmeleri olduğu değerlendirildi. Tahsilat 1'deki zamanında/geç ödeme sütunlarındaki boşluklar ise rastgele değil sistematik — her kayıt sadece ödendiği zaman dilimine ait sütunu dolduruyor.
IQR yöntemiyle 48,554 aşırı uç değer (%4.1) tespit edildi, %99.8'i yüksek tüketim kaynaklı (ticari/tarımsal/kamu hesapları orantısız fazla temsil ediliyor).
Müşteriler tüketim ve ödeme davranışına göre 4 segmente ayrıldı: Değerli Sadık Müşteri, Sadık Düşük Tüketici, Riskli Yüksek Tüketici, Riskli Düşük Tüketici — ödeme davranışı belirgin şekilde iki kutuplu (ya çok güvenilir ya yüksek riskli).
(Bonus) Geç ödeme oranını en çok etkileyen faktör hesap sınıfı: Belediye (%59.7) ve Köy İçme Suyu/Tarımsal Kooperatif gibi kamu/tarım altyapı hesapları en riskli ödeyiciler, beklenenin aksine Mesken (%26.7) ortalamanın altında kalıyor. İlçe farkı görece küçük (%24.8-%30.8). Fatura tutarına göre "U şeklinde" bir örüntü var: hem çok küçük hem çok büyük faturalar orta tutarlı faturalara göre daha sık geç ödeniyor.
Bu bulgular; riskli müşteri gruplarına göre tahsilat kaynaklarının önceliklendirilmesini, mevsimsel talebe göre kapasite planlamasını ve düşük riskli müşterilere sadakat teşvikleri sunulmasını mümkün kılıyor.
Kurulum ve Çalıştırma

Notebook'lar Google Colab üzerinde çalıştırılmak üzere hazırlanmıştır.

elektrik_veri_hashed.xlsx dosyasını Google Drive'a yükleyin.
Notebook'u açıp ilk hücrede Drive'ı bağlayın (from google.colab import drive; drive.mount(...)).
Gerekli kütüphaneleri kurun:
pip install -r requirements.txt
Hücreleri sırasıyla çalıştırın.
