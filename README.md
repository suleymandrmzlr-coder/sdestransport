# SDES Logistics — bağımsız Hostinger CMS

Önce **HOSTINGER-KURULUM.md** dosyasını okuyun. Paket Node.js 24 + MariaDB için hazırlanmıştır. `public_html` içine statik olarak çıkarmak yönetim ve veritabanını çalıştırmaz.

- Yönetim: `/cms`; eski operasyon paneli: `/admin-tr.html`.
- Tam veritabanı içe aktarımı: `database/sdes-initial.sql`.
- Şema: `database/schema-mariadb.sql`; mevcut kayıtların taşınabilir kopyası: `database/live-records.json`.
- Hazır yönetici şifresi yoktur. İlk hesabı HTTPS üzerinden kurulum anahtarıyla oluşturun.
- Ana dil İngilizce; İspanyolca, Türkçe, Almanca ve eklenebilir diller. Eksik yeni dil çevirileri İngilizceye döner.
- İçerik, görsel, sosyal medya, SEO, ölçüm bağlantıları ve çalışan izinleri panelden yönetilir.
- Yayındaki eski siteye/DNS'e müdahale edilmedi. Gerçek Hostinger/MariaDB ortamında kabul testini tamamlayın.

## Geliştirme

```bash
npm ci --include=dev
npm run build
npm test
npm start
```

`npm start` için DB değişkenleri gerekir. `npm test` izole SQLite test adaptörü kullanır; MariaDB entegrasyon testinin yerine geçmez. Kaynaklar ve test raporu pakete dahildir.

## Mimari ve devam

`server/`: Express sınırı, oturumlar, yetkiler, CMS, sunucuda HTML üretimi ve MariaDB adaptörü.
`lib/`: lojistik iş kuralları, takip, operasyonlar, finans ve PDF üretimi.
`public/`: sayfa şablonları, tasarım, görseller ve tarayıcı kodu.
`source/`: varsayılan içerik/sözlük JSON kaynakları.
`dist-public/`: derleme sırasında küçültülen CSS/JS.
`drizzle/`: eski şema geçmişi; izole regresyon testlerinde kullanılır. Hostinger'de bu SQLite SQL dosyalarını çalıştırmayın.

Sayfa şablonlarındaki `data-cms-node` kimliklerini koruyun. Panel verileri bu sabit anahtarlara bağlıdır. Şablon eklerken mevcut kimlikleri yeniden numaralandırmayın. CMS içerikleri MariaDB'de tutulur; kaynak ZIP tek başına sonraki canlı değişikliklerin yedeği değildir.

Kaynak devam noktası: eski SDES commit `65c09052d2bef42c9da3f1863ecdb81ea2066a23` üzerine bağımsız geçiş. Eski Cloudflare/Vinext dağıtım dosyaları final pakete alınmadı. Bu paketi eski Sites ortamına Worker olarak yayımlamayın.
