# FBref Süper Lig Takım İstatistikleri Kazıyıcı (Scraper)

Bu Python betiği, `fbref.com` adresindeki Süper Lig takımlarının detay sayfalarından kapsamlı istatistik verilerini çekmek için tasarlanmıştır. Selenium ile web sayfalarını otomatik olarak ziyaret eder, BeautifulSoup ile HTML içeriğini ayrıştırır ve her bir tabloyu düzenli bir dosya yapısı altında CSV dosyalarına kaydeder.

## Özellikler

*   **Kapsamlı Veri Çekimi**: Süper Lig'deki her bir takımın standart istatistikleri, maç logları, kalecilik, şut, oyun süresi ve çeşitli istatistikler dahil olmak üzere tüm önemli tabloları kazır.
*   **Düzenli Dosyalama**: Çekilen veriler `scraped_fbref_team_stats/[Takım Adı]/[Tablo Adı]_[YYYY-MM-DD].csv` formatında klasörlenir ve kaydedilir.
*   **Çok Seviyeli Başlık Düzleştirme**: Pandas'ın `MultiIndex` sütun başlıklarını tek bir okunabilir satıra dönüştürerek veri erişimini kolaylaştırır.
*   **Haftalık Otomasyon Uyumluluğu**: Dosya adlarına eklenen tarih, haftalık çalıştırmalar için uygundur ve geçmiş verileri korur.
*   **Bot Korumasını Aşma**: Selenium kullanarak `fbref.com` gibi sitelerin bot koruma mekanizmalarını aşar.
*   **Hata Yönetimi**: Ağ hataları, tablo bulunamaması gibi durumlar için sağlam hata yönetimi içerir.

## Ön Gereksinimler

Kodu kendi yerel makinenizde veya bir sunucuda çalıştırmadan önce aşağıdaki adımları tamamlamanız gerekmektedir:

1.  **Python Kurulumu**: Python 3.x'in sisteminizde yüklü olduğundan emin olun.
2.  **Gerekli Kütüphaneler**: Terminalinizde aşağıdaki komutu çalıştırarak tüm Python bağımlılıklarını kurun:
    ```bash
    pip install selenium pandas beautifulsoup4 lxml webdriver_manager
    ```
3.  **Chrome Tarayıcı Kurulumu**: Google Chrome tarayıcısının sisteminizde kurulu olması gerekir. `webdriver_manager` Chrome sürücüsünü otomatik olarak yöneteceğinden manuel indirme yapmanıza gerek yoktur.
    *   **Sunucu Ortamında (örneğin Linux)**: Headless Chrome'u kullanabilmek için sunucunuzda Chrome/Chromium'un kurulu olması gerekir. Örneğin, Ubuntu/Debian tabanlı sistemler için:
        ```bash
        sudo apt-get update
        sudo apt-get install -y chromium-browser chromium-chromedriver
        ```
        veya Google Chrome için:
        ```bash
        wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
        sudo dpkg -i google-chrome-stable_current_amd64.deb
        sudo apt-get install -f # Eksik bağımlılıkları yüklemek için
        ```

## Kullanım

1.  **Kodu Kaydedin**: Yukarıdaki Python kodunu `fbref_scraper.py` gibi bir isimle kaydedin.
2.  **`team_list`'i Doğrulayın**: Kod içinde tanımlı `team_list` sözlüğünün Süper Lig'deki tüm 18 takımı doğru `team_id` ve `Team-Stats` URL'leri ile içerdiğinden emin olun. (Kodda halihazırda 18 takımın genel bir listesi bulunmaktadır.)
3.  **Çalıştırın**: Terminalinizi kullanarak dosyanın kaydedildiği dizine gidin ve şu komutu çalıştırın:
    ```bash
    python fbref_scraper.py
    ```

Kod çalıştığında, mevcut dizinde `scraped_fbref_team_stats` adında bir klasör oluşturacak ve altına her takım için ayrı klasörler açarak ilgili istatistik tablolarını CSV dosyaları olarak kaydedecektir.

## Çekilen Tablolar ve Veri Açıklamaları

Betiğin her takımın detay sayfasından çektiği başlıca tablolar ve içerdikleri sütunların açıklamaları aşağıdadır:

Elbette, projenizin README dosyası için istediğiniz açıklamaların Türkçe karşılıkları ve açıklamaları aşağıda başlıklar halinde düzenlenmiştir.

---

### **Süper Lig Puan Durumu (Süper Lig Table)**

*   **Rk (Sıra):** Takımın ligdeki sıralaması.
*   **Squad finish in competition (Takımın turnuvadaki yeri):** Takımın lig veya turnuva içindeki bitiş pozisyonu. Eleme usulü turnuvalarda ulaşılan son turu gösterebilir.
*   **Colors and arrows (Renkler ve oklar):** Bir üst lige yükselmeyi/bir alt lige düşmeyi veya kıtasal kupalara (Şampiyonlar Ligi, Avrupa Ligi vb.) katılım hakkını temsil eder.
*   **Trophy (Kupa Simgesi):** Takımın, playoff veya ligi lider bitirerek şampiyon olduğunu gösterir.
*   **Star (Yıldız Simgesi):** Şampiyonun başka bir yöntemle (örneğin playoff) belirlendiği liglerde, normal sezonu lider bitiren takımı gösterir.
*   **MP (OM - Oynanan Maç):** Takımın oynadığı toplam maç sayısı.
*   **W (G - Galibiyet):** Kazanılan maç sayısı.
*   **D (B - Beraberlik):** Berabere biten maç sayısı.
*   **L (M - Mağlubiyet):** Kaybedilen maç sayısı.
*   **GF (AG - Atılan Gol):** Takımın attığı toplam gol sayısı.
*   **GA (YG - Yenilen Gol):** Takımın yediği toplam gol sayısı.
*   **GD (AV - Averaj):** Atılan gol ile yenilen gol arasındaki fark (AG - YG).
*   **Pts (P - Puan):** Takımın topladığı toplam puan. (Galibiyet için 3, beraberlik için 1 puan).
*   **Pts/MP (MBP - Maç Başına Puan):** Oynanan maç başına kazanılan puan ortalaması.
*   **Last 5 (Son 5 Maç):** Takımın oynadığı son beş maçın sonucu (soldan sağa kronolojik olarak).
*   **Attendance (Seyirci Ortalaması):** Sezon boyunca iç saha maçlarındaki maç başına ortalama seyirci sayısı.
*   **Top Team Scorer (Takımın Gol Kralı):** Sadece o sezonki lig maçlarında takım adına en çok gol atan oyuncu.
*   **Goalkeeper (Kaleci):** Ligde en fazla dakika oynayan kaleci.

### **Kaleci İstatistikleri (Goalkeeping)**

*   **# Pl (Oyuncu Sayısı):** Maçlarda kullanılan toplam oyuncu sayısı.

**Oyun Süresi (Playing Time)**
*   **MP (OM - Oynanan Maç):** Kalecinin oynadığı maç sayısı.
*   **Starts (İlk 11):** Kalecinin ilk 11'de başladığı maç sayısı.
*   **Min (Dakika):** Oynanan toplam dakika.
*   **90s (90 Dakika):** Oynanan toplam dakikanın 90'a bölünmüş hali.

**Performans (Performance)**
*   **GA (YG - Yenilen Gol):** Kalesinde gördüğü toplam gol sayısı.
*   **GA90 (90 Dakika Başına Yediği Gol):** 90 dakika başına kalesinde gördüğü ortalama gol sayısı.
*   **SoTA (Kalesine Gelen İsabetli Şut):** Rakip takımın kaleyi bulan toplam şut sayısı.
*   **Save% (Kurtarış Yüzdesi):** Yapılan kurtarışların, kaleye gelen isabetli şutlara oranı. `(Kalesine Gelen İsabetli Şut - Yediği Gol) / Kalesine Gelen İsabetli Şut`. Penaltı vuruşlarını içermez.
*   **W, D, L (Galibiyet, Beraberlik, Mağlubiyet):** Kalecinin oynadığı maçlardaki galibiyet, beraberlik ve mağlubiyet sayıları.
*   **CS (Gol Yemediği Maç):** Kalecinin gol yemeden tamamladığı maç sayısı (Clean Sheet).
*   **CS% (Gol Yememe Yüzdesi):** Gol yemeden tamamlanan maçların, oynanan toplam maça oranı.

**Penaltı Vuruşları (Penalty Kicks)**
*   **PKatt (Kullanılan Penaltı - Rakip):** Rakibin kullandığı toplam penaltı vuruşu sayısı.
*   **PKA (Yenilen Penaltı Golü):** Penaltıdan yenilen gol sayısı.
*   **PKsv (Kurtarılan Penaltı):** Kalecinin kurtardığı penaltı sayısı.
*   **PKm (Kaçırılan Penaltı - Rakip):** Rakibin kaçırdığı (kaleyi bulmayan veya dışarı giden) penaltı sayısı.
*   **Save% (Penaltı Kurtarış Yüzdesi):** Kurtarılan penaltıların, kullanılan toplam penaltı sayısına oranı.

### **Takım Şut İstatistikleri (Squad Shooting)**

*   **# Pl (Oyuncu Sayısı):** Maçlarda kullanılan toplam oyuncu sayısı.
*   **90s (90 Dakika):** Oynanan toplam dakikanın 90'a bölünmüş hali.

**Standart (Standard)**
*   **Gls (Gol):** Atılan toplam gol sayısı.
*   **Sh (Toplam Şut):** Çekilen toplam şut sayısı (penaltılar hariç).
*   **SoT (İsabetli Şut):** Kaleyi bulan şut sayısı (penaltılar hariç).
*   **SoT% (İsabetli Şut Yüzdesi):** İsabetli şutların toplam şut sayısına oranı (%).
*   **Sh/90 (90 Dakika Başına Şut):** 90 dakika başına çekilen ortalama şut sayısı.
*   **SoT/90 (90 Dakika Başına İsabetli Şut):** 90 dakika başına çekilen ortalama isabetli şut sayısı.
*   **G/Sh (Şut Başına Gol):** Çekilen her şutun gol olma oranı.
*   **G/SoT (İsabetli Şut Başına Gol):** Kaleyi bulan her şutun gol olma oranı.
*   **Dist (Ortalama Şut Mesafesi):** Çekilen tüm şutların kaleye olan ortalama uzaklığı (yarda cinsinden, penaltılar hariç).
*   **PK (Atılan Penaltı Golü):** Golle sonuçlanan penaltı vuruşları.
*   **PKatt (Kullanılan Penaltı):** Takımın kullandığı toplam penaltı vuruşu sayısı.

### **Takım Oyun Süreleri (Squad Playing Time)**

*   **# Pl (Oyuncu Sayısı):** Maçlarda kullanılan toplam oyuncu sayısı.
*   **Age (Ortalama Yaş):** Oynanan dakikalara göre ağırlıklandırılmış yaş ortalaması.

**Oyun Süresi (Playing Time)**
*   **MP (OM - Oynanan Maç):** Oyuncunun oynadığı maç sayısı.
*   **Min (Dakika):** Oynanan toplam dakika.
*   **Mn/MP (Maç Başına Dakika):** Oynanan maç başına düşen ortalama dakika.
*   **Min% (Takım Dakikalarındaki Yüzdesi):** Oyuncunun sahada olduğu sürenin, takımın toplam oynadığı süreye oranı (%).
*   **90s (90 Dakika):** Oynanan toplam dakikanın 90'a bölünmüş hali.

**İlk 11 (Starts)**
*   **Starts (İlk 11 Başlama):** Oyuncunun ilk 11'de başladığı maç sayısı.
*   **Mn/Start (İlk 11 Başladığı Maç Başına Dakika):** İlk 11'de başlanan maç başına oynanan ortalama dakika.
*   **Compl (Tamamladığı Maç Sayısı):** Oyuncunun baştan sona oynadığı maç sayısı.

**Yedek (Subs)**
*   **Subs (Oyuna Sonradan Girme):** Oyuncunun yedekten oyuna girdiği maç sayısı.
*   **Mn/Sub (Yedekten Girilen Maç Başına Dakika):** Oyuna sonradan dahil olunduğunda oynanan ortalama dakika.
*   **unSub (Kullanılmadığı Yedek Maç):** Yedek kulübesinde olup oyuna girmediği maç sayısı.

**Takım Başarısı (Oyuncu Sahadayken) (Team Success)**
*   **PPM (Maç Başına Puan):** Oyuncunun forma giydiği maçlarda takımın kazandığı puan ortalaması.
*   **onG (Sahadayken Atılan Gol):** Oyuncu sahadayken takımın attığı gol sayısı.
*   **onGA (Sahadayken Yenilen Gol):** Oyuncu sahadayken takımın yediği gol sayısı.
*   **+/- (Artı/Eksi):** Oyuncu sahadayken takımın attığı gol ile yediği gol arasındaki fark.
*   **+/-90 (90 Dakika Başına Artı/Eksi):** Oyuncunun oynadığı her 90 dakika başına takımın artı/eksi değeri.

### **Takım Diğer İstatistikler (Squad Miscellaneous Stats)**

*   **# Pl (Oyuncu Sayısı):** Maçlarda kullanılan toplam oyuncu sayısı.
*   **90s (90 Dakika):** Oynanan toplam dakikanın 90'a bölünmüş hali.

**Performans (Performance)**
*   **CrdY (Sarı Kart):** Görülen sarı kart sayısı.
*   **CrdR (Kırmızı Kart):** Görülen kırmızı kart sayısı.
*   **2CrdY (İkinci Sarıdan Kırmızı):** İkinci sarı karttan dolayı görülen kırmızı kart sayısı.
*   **Fls (Yapılan Faul):** Yapılan toplam faul sayısı.
*   **Fld (Maruz Kalınan Faul):** Kazanılan (rakibin yaptığı) faul sayısı.
*   **Off (Ofsayt):** Düşülen ofsayt sayısı.
*   **Crs (Orta):** Yapılan orta sayısı.
*   **Int (Top Kesme):** Rakibin pasını kesme veya engelleme sayısı.
*   **TklW (Kazanılan Müdahale):** Yapılan müdahale sonucunda topun takımda kaldığı başarılı müdahale sayısı.
*   **PKwon (Kazanılan Penaltı):** Takımın lehine kazanılan penaltı sayısı.
*   **PKcon (Sebep Olunan Penaltı):** Takımın aleyhine neden olunan penaltı sayısı.
*   **OG (Kendi Kalesine Gol):** Oyuncunun kendi kalesine attığı gol sayısı.

### 1. Player Standard Stats (Oyuncu Standart İstatistikleri)

Bu tablo, takımdaki her oyuncunun genel performansını özetler.

*   **Player**: Oyuncunun adı.
*   **Nation**: Oyuncunun milliyeti.
*   **Pos**: Oyuncunun en çok oynadığı pozisyon.
    *   GK - Kaleciler
    *   DF - Defans oyuncuları
    *   MF - Orta saha oyuncuları
    *   FW - Forvet oyuncuları
    *   FB - Bekler
    *   (ve diğer spesifik pozisyonlar: LB, RB, CB, DM, CM, LM, RM, WM, LW, RW, AM)
*   **Age**: Sezon başlangıcındaki yaşı.
*   **Playing Time - MP**: Oynadığı Maç Sayısı.
*   **Playing Time - Starts**: Başladığı Maç Sayısı.
*   **Playing Time - Min**: Oynadığı Dakika.
*   **Playing Time - 90s**: Oynadığı 90 dakikalık birim sayısı (Toplam dakika / 90).
*   **Performance - Gls**: Attığı Gol Sayısı.
*   **Performance - Ast**: Yaptığı Asist Sayısı.
*   **Performance - G+A**: Gol + Asist Toplamı.
*   **Performance - G-PK**: Penaltıdan Olmayan Gol Sayısı.
*   **Performance - PK**: Attığı Penaltı Golü Sayısı.
*   **Performance - PKatt**: Kullandığı Penaltı Sayısı.
*   **Performance - CrdY**: Gördüğü Sarı Kart Sayısı.
*   **Performance - CrdR**: Gördüğü Kırmızı Kart Sayısı.
*   **Per 90 Minutes - Gls**: 90 Dakika Başına Gol Sayısı.
*   **Per 90 Minutes - Ast**: 90 Dakika Başına Asist Sayısı.
*   **Per 90 Minutes - G+A**: 90 Dakika Başına Gol + Asist Sayısı.
*   **Per 90 Minutes - G-PK**: 90 Dakika Başına Penaltısız Gol Sayısı.
*   **Per 90 Minutes - G+A-PK**: 90 Dakika Başına Penaltısız Gol + Asist Sayısı.
*   **Matches**: Oyuncunun maç loglarına bağlantı.

### 2. Match Logs (Maç Logları) - All Competitions, Super Lig, Champions League

Bu tablolar, takımın tüm turnuvalardaki (veya spesifik Süper Lig/Şampiyonlar Ligi gibi) maçlarının detaylarını içerir.

*   **Date**: Maçın yerel tarihi.
*   **Time**: Maçın yerel başlama saati.
*   **Comp**: Maçın oynandığı turnuva.
*   **Round**: Turnuva turu veya aşaması.
*   **Day**: Haftanın günü.
*   **Venue**: Maçın oynandığı yer (Ev Sahibi/Deplasman).
*   **Result**: Maç sonucu (W-Galibiyet, D-Beraberlik, L-Mağlubiyet).
*   **Goals For - GF**: Takımın attığı gol sayısı.
*   **Goals Against - GA**: Takımın yediği gol sayısı.
*   **Opponent**: Rakip takım.
*   **Possession - Poss**: Topa sahip olma yüzdesi.
*   **Attendance**: Maç seyirci sayısı.
*   **Captain**: Takım kaptanı.
*   **Formation**: Takımın kullandığı diziliş.
*   **Opp Formation**: Rakip takımın kullandığı diziliş.
*   **Referee**: Maçın hakemi.
*   **Match Report**: Maç raporuna bağlantı.
*   **Notes**: Maçla ilgili ek notlar.

### 3. Player Goalkeeping Stats (Oyuncu Kalecilik İstatistikleri)

Takımdaki kalecilerin performansını gösteren tablo.

*   **Player**: Kalecinin adı.
*   **Nation**: Kalecinin milliyeti.
*   **Pos**: Kalecinin pozisyonu (GK).
*   **Age**: Sezon başlangıcındaki yaşı.
*   **Playing Time - MP**: Oynadığı Maç Sayısı.
*   **Playing Time - Starts**: Başladığı Maç Sayısı.
*   **Playing Time - Min**: Oynadığı Dakika.
*   **Playing Time - 90s**: Oynadığı 90 dakikalık birim sayısı.
*   **Performance - GA**: Yediği Gol Sayısı.
*   **Performance - GA90**: 90 Dakika Başına Yediği Gol Sayısı.
*   **Performance - SoTA**: Kaleye Gelen İsabetli Şut Sayısı.
*   **Performance - Saves**: Kurtarış Sayısı.
*   **Performance - Save%**: Kurtarış Yüzdesi.
*   **Performance - W**: Galibiyet sayısı.
*   **Performance - D**: Beraberlik sayısı.
*   **Performance - L**: Mağlubiyet sayısı.
*   **Performance - CS**: Gol Yemediği Maç (Clean Sheet) Sayısı.
*   **Performance - CS%**: Gol Yemediği Maç Yüzdesi.
*   **Penalty Kicks - PKatt**: Kaleye Gelen Penaltı Sayısı.
*   **Penalty Kicks - PKA**: Penaltıdan Yenilen Gol Sayısı.
*   **Penalty Kicks - PKsv**: Kurtardığı Penaltı Sayısı.
*   **Penalty Kicks - PKm**: Kaçırdığı Penaltı Sayısı.
*   **Penalty Kicks - Save%**: Penaltı Kurtarış Yüzdesi.
*   **Matches**: Kalecinin maç loglarına bağlantı.

### 4. Player Shooting Stats (Oyuncu Şut İstatistikleri)

Takımdaki oyuncuların şut performansını gösteren tablo.

*   **Player**: Oyuncunun adı.
*   **Nation**: Oyuncunun milliyeti.
*   **Pos**: Oyuncunun pozisyonu.
*   **Age**: Sezon başlangıcındaki yaşı.
*   **90s**: Oynadığı 90 dakikalık birim sayısı.
*   **Standard - Gls**: Attığı Gol Sayısı.
*   **Standard - Sh**: Toplam Şut Sayısı (Penaltılar hariç).
*   **Standard - SoT**: Kaleye İsabetli Şut Sayısı (Penaltılar hariç).
*   **Standard - SoT%**: İsabetli Şut Yüzdesi.
*   **Standard - Sh/90**: 90 Dakika Başına Toplam Şut Sayısı.
*   **Standard - SoT/90**: 90 Dakika Başına Kaleye İsabetli Şut Sayısı.
*   **Standard - G/Sh**: Şut Başına Gol Sayısı.
*   **Standard - G/SoT**: İsabetli Şut Başına Gol Sayısı.
*   **Standard - Dist**: Ortalama Şut Mesafesi (yarda, penaltılar hariç).
*   **Standard - PK**: Attığı Penaltı Golü Sayısı.
*   **Standard - PKatt**: Kullandığı Penaltı Sayısı.
*   **Matches**: Oyuncunun maç loglarına bağlantı.

### 5. Player Playing Time Stats (Oyuncu Oyun Süresi İstatistikleri)

Takımdaki oyuncuların maç ve süre bilgilerini gösteren tablo.

*   **Player**: Oyuncunun adı.
*   **Nation**: Oyuncunun milliyeti.
*   **Pos**: Oyuncunun pozisyonu.
*   **Age**: Sezon başlangıcındaki yaşı.
*   **Playing Time - MP**: Oynadığı Maç Sayısı.
*   **Playing Time - Min**: Oynadığı Dakika.
*   **Playing Time - Mn/MP**: Maç Başına Oynadığı Dakika.
*   **Playing Time - Min%**: Takım dakikalarının yüzde kaçında sahada olduğu.
*   **Playing Time - 90s**: Oynadığı 90 dakikalık birim sayısı.
*   **Starts - Starts**: Başladığı Maç Sayısı.
*   **Starts - Mn/Start**: Başladığı Maç Başına Dakika.
*   **Starts - Compl**: Tamamladığı Maç Sayısı.
*   **Subs - Subs**: Oyuna Sonradan Girdiği Maç Sayısı.
*   **Subs - Mn/Sub**: Yedekten Girdiği Maç Başına Dakika.
*   **Subs - unSub**: Yedekte Kaldığı ancak Oyuna Girmediği Maç Sayısı.
*   **Team Success - PPM**: Oynadığı Maç Başına Takımın Aldığı Puan Ortalaması.
*   **Team Success - onG**: Sahadayken Takımın Attığı Gol Sayısı.
*   **Team Success - onGA**: Sahadayken Takımın Yediği Gol Sayısı.
*   **Team Success - +/-**: Sahadayken Takımın Gol Farkı (Attığı - Yediği).
*   **Team Success - +/-90**: 90 Dakika Başına Gol Farkı.
*   **Team Success - On-Off**: Sahadayken takımın net gol farkı ile sahada değilken takımın net gol farkı arasındaki fark (90 dakika başına).
*   **Matches**: Oyuncunun maç loglarına bağlantı.

### 6. Player Miscellaneous Stats (Oyuncu Çeşitli İstatistikler)

Takımdaki oyuncuların diğer çeşitli istatistiklerini gösteren tablo.

*   **Player**: Oyuncunun adı.
*   **Nation**: Oyuncunun milliyeti.
*   **Pos**: Oyuncunun pozisyonu.
*   **Age**: Sezon başlangıcındaki yaşı.
*   **90s**: Oynadığı 90 dakikalık birim sayısı.
*   **Performance - CrdY**: Gördüğü Sarı Kart Sayısı.
*   **Performance - CrdR**: Gördüğü Kırmızı Kart Sayısı.
*   **Performance - 2CrdY**: İkinci Sarı Karttan Gördüğü Kırmızı Kart Sayısı.
*   **Performance - Fls**: Yaptığı Faul Sayısı.
*   **Performance - Fld**: Faul Aldığı Sayı (faul yapılan).
*   **Performance - Off**: Ofsayt Sayısı.
*   **Performance - Crs**: Yaptığı Orta Sayısı.
*   **Performance - Int**: Top Kesme Sayısı.
*   **Performance - TklW**: Kazandığı Top Kapma Sayısı.
*   **Performance - PKwon**: Kazanılan Penaltı Sayısı.
*   **Performance - PKcon**: Neden Olduğu Penaltı Sayısı.
*   **Performance - OG**: Kendi Kalesine Attığı Gol Sayısı.
*   **Matches**: Oyuncunun maç loglarına bağlantı.

---
