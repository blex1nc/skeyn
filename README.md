# Skeyn

Yerel ağındaki bilgisayarların belleğini birleştirerek tek makineye sığmayan MoE dil modellerini çalıştıran masaüstü uygulaması. Tek bilgisayarda da tam çalışır; ikinci bir cihaz eklemek isteğe bağlı bir güçlendirmedir.

skeyn.ai · Blex Enterprises / Incord, Ltd.

## İndir

Son sürüm: **[Releases → Latest](https://github.com/blex1nc/skeyn/releases/latest)**

| Platform | Dosya |
|---|---|
| macOS 13+ (Apple Silicon ve Intel) | `Skeyn_<sürüm>_universal.dmg` |
| Windows 10/11 (x64) | `Skeyn_<sürüm>_x64-setup.exe` |

Uygulama yeni sürümleri kendisi denetler ve güncellemeden önce sorar.

## Kurulum

### macOS (Apple Silicon ve Intel)

1. `Skeyn_<sürüm>_universal.dmg` dosyasını aç ve Skeyn'i **Uygulamalar** klasörüne sürükle.
2. Uygulama henüz imzalı değil. İlk açılışta macOS "geliştirici doğrulanamadı" diyebilir:
   - Uygulamalar'da Skeyn'e **sağ tıkla → Aç → Aç**, ya da
   - Terminal: `xattr -dr com.apple.quarantine /Applications/Skeyn.app`
3. Skeyn yerel ağdaki cihazları bulmak için izin ister. İkinci bir bilgisayar kullanacaksan **İzin Ver**'i seç.

Gereksinim: macOS 13 veya üzeri.

### Windows (x64)

1. `Skeyn_<sürüm>_x64-setup.exe` dosyasını çalıştır. Kurulum yalnızca senin kullanıcı hesabına yapılır; yönetici yetkisi gerekmez.
2. Kurulum dosyası henüz imzalı değil. SmartScreen uyarısında **Ek bilgi → Yine de çalıştır**'ı seç.
3. Windows Güvenlik Duvarı sorarsa **özel ağlar** için izin ver (cihazların birbirini bulması için).

Gereksinim: Windows 10 veya 11, x64.

### İlk açılış

Uygulama üç adımlı bir kurulumla başlar:

1. **Donanım taraması** — bellek ve bellek bant genişliği yaklaşık beş saniyede ölçülür. Hız tahminleri bu ölçüme dayanır.
2. **Model önerisi** — bilgisayarına sığan en uygun model önerilir. Hemen indirmeye başlayabilir ya da (Pro ve Max planlarında) elindeki bir GGUF dosyasını içe aktarabilirsin.
3. **İkinci cihaz** — isteğe bağlı, atlanabilir.

Model bir kez indirildikten sonra her şey internetsiz çalışır. Yazdıkların cihazlarından çıkmaz.

### Nereye ne kaydedilir

| | macOS | Windows |
|---|---|---|
| Modeller ve sohbetler | `~/Library/Application Support/ai.skeyn.desktop/` | `%APPDATA%\ai.skeyn.desktop\` |
| Ayarlar ve eşleşmeler | `~/Library/Application Support/ai.skeyn.desktop/` | `%APPDATA%\ai.skeyn.desktop\` |

Veri klasörü Ayarlar'dan değiştirilebilir. Eşleşme bilgileri şifreli saklanır; şifreleme anahtarı işletim sisteminin anahtar zincirindedir.

---

## Uygulama

### Sohbet

- Markdown, kod blokları ve kopyalama, akış halinde yanıt.
- Yanıtı yeniden üretme, durdurma, kopyalama.
- Her yanıtın altında **gerçek hız ile tahmini hızın karşılaştırması** (tok/s).
- Giriş kutusunda hangi modelin, hangi cihazda, ne hızda yanıt verdiği görünür.
- Sohbet arama, yeniden adlandırma, silme. ⌘N / Ctrl+N yeni sohbet.

### Modeller

Seçilmiş MoE modelleri listelenir. Her modelin yanında, **indirmeden önce** şunlar görünür:

- boyut, toplam ve aktif parametre sayısı,
- ne kadar bellek gerektirdiği ve cihazlarındaki bellekle karşılaştırması,
- mevcut cihazlarında **tahmini hız** ve kaç cihaza ihtiyaç duyduğu.

| Hız | Anlamı |
|---|---|
| 12+ tok/s | Okuma hızının üstünde |
| 7–12 tok/s | Rahat |
| 4–7 tok/s | Yavaş ama kullanılabilir |
| < 4 tok/s | Kullanışsız |

İndirmeler duraklatılıp devam ettirilebilir, iptal edilebilir ve sağlama toplamıyla doğrulanır. Pro ve Max planlarında kendi GGUF dosyanı da içe aktarabilirsin; dense bir model seçersen Skeyn engellemez, ama aynı boyuttaki MoE modelle hız karşılaştırmasını gösterir.

Başlangıç listesi: OLMoE 1B-7B, gpt-oss-20b, Qwen3-30B-A3B, Qwen3.6-35B-A3B, Mixtral 8x7B, gpt-oss-120b, Qwen3-235B-A22B, DeepSeek-V3.

### Birden fazla cihaz

Üst çubuktaki cihaz sayısına tıklayınca (⌘/ / Ctrl+/) cihaz paneli açılır:

- her cihazın bellek kullanımı canlı olarak, bağlantı türü ve gecikmesi,
- modelin katmanlarının hangi cihazda durduğunu gösteren **katman planı**,
- yeni cihaz ekleme.

**Eşleştirme:** Bir bilgisayarda **Eşleşme kodu göster**'i seç; diğerinde **Kod gir** ile altı haneli kodu yaz. IP adresi girmen gerekmez — aynı ağdaki cihazlar otomatik bulunur. Bir kez eşleşen cihazlar sonraki açılışlarda kendiliğinden bağlanır; panelden **Unut** ile kaldırılabilir. Otomatik bulma çalışmayan ağlarda **IP adresiyle bağlan** seçeneği vardır; Tailscale kuruluysa tailnet'teki cihazlar da bulunur.

**Model yükleme:** Model tek bilgisayara sığıyorsa yalnızca o bilgisayarda çalışır. Sığmıyorsa Skeyn modeli, sığdığı en az sayıda cihaza katman katman böler. Bir cihazın bağlantısı koparsa model kalan cihazlara yeniden planlanır.

**Güvenlik:** Cihazlar arasındaki tüm trafik şifrelidir (Noise, ChaCha20-Poly1305). Eşleşme kodu ağa gönderilmez (SPAKE2); yanlış kod denemeleri sınırlanır ve kilitlenir. Bir bilgisayarın belleği yalnızca onun izin verdiği eşleşmiş cihazlarca kullanılabilir; Ayarlar'dan kapatılabilir.

### Ayarlar

Dil (Türkçe / English), cihaz adı, bağlam uzunluğu, speculative decoding için taslak model, koordinatör seçimi, bellek paylaşımı, uzaktan yükleme izni, Tailscale, veri klasörü, donanım bilgisi ve yeniden ölçüm, günlük kayıtları.

### Telefon

iOS projesi hazırdır: telefon model çalıştırmaz, eşleştiği bilgisayardaki modelle sohbet eder. Henüz dağıtılmıyor.
