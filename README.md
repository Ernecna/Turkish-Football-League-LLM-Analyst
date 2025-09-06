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
