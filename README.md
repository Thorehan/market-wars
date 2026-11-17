# Market Wars: Oynanabilir Prototip

Aynı restoranda iki oyuncunun 3 dakika boyunca en çok parayı kazanmak için yarıştığı, joystick ile oynanan dikey ekran oyununun test prototipi. Tek dosyadır (`index.html`). Kurulum, sunucu ya da ücret gerekmez.

## Yerelde oynamak

`index.html` dosyasına çift tıkla, tarayıcıda açılır.

Telefonda denemek için aşağıdaki GitHub Pages adımlarıyla bir link oluştur ve o linki telefonda aç.

## GitHub'da ücretsiz yayınlamak (GitHub Pages)

1. github.com'da yeni bir **public** repo oluştur (ör. `market-wars-prototype`).
2. **Add file → Upload files** ile `index.html` ve `README.md` dosyalarını yükle, **Commit changes** de.
3. Repo'da **Settings → Pages** bölümüne git. *Source* olarak **Deploy from a branch**, branch olarak **main** ve **/(root)** seç, **Save** de.
4. 1–2 dakika sonra oyun şu adreste yayında olur: `https://<kullanıcı-adın>.github.io/market-wars-prototype/`

Bu linki telefonda açıp "Ana ekrana ekle" dersen uygulama gibi tam ekran açılır.

## Modlar

| Mod | Açıklama |
|---|---|
| Bot'a karşı | Sen Mavi'sin (solda), bot Kırmızı (sağda). |
| 2 kişi, aynı cihaz | Ekranın sol yarısına dokunan Mavi'yi, sağ yarısına dokunan Kırmızı'yı yönetir. Klavyede WASD ve Oklar. |
| Bot vs Bot izle | Dengeyi gözlemlemek için iki bot oynar. |

## Kontroller

- **Telefon:** ekrana dokun ve sürükle (sanal joystick)
- **Bilgisayar:** WASD / ok tuşları; P veya Esc duraklatır

## Oyun döngüsü (v0.2.1)

1. **Dolap:** 1,25 sn'de 1 çiğ et üretir. Üstünden geçince etler kucağına akar.
2. **Izgara:** çiğ etleri üstüne bırakırsın; aynı anda 4 et, 3 sn'de pişer.
3. **Bant:** pişen et kendiliğinden servis tezgâhına kayar.
4. **Servis tezgâhı:** tek sıradaki müşteriler, boş olan ve eti olan kasaya gider; ikisi de boşsa **daha dolu tezgâha** gider. Dükkânı uzun süre bırakırsan rakibin tezgâhı seninkini geçer ve müşteriler ona akar.
5. **Kasa:** müşterinin bıraktığı parayı üstünden geçerek toplarsın. **Skor = toplanan para.**

Senin işin üç durak: **dolap, ızgara, kasa.** Kalan zaman sabotaja kalır.

## Sabotaj kutusu

Her 15 sn'de restoranın ortasına bir kutu düşer (5 çeşit, eşit ihtimalle). Nereye düşeceği ve içinde ne olduğu 3 sn önceden yerde görünür. İlk dokunan alır ve etki rakibe kendiliğinden uygulanır. Verilecek tek karar: "Koşayım mı?"

| Kutu | Etki | Karşı hamle |
|---|---|---|
| 🐀 Fare | Rakibin kasasındaki parayı her 0,5 sn'de $10 kemirir (8 sn). | Sahibi kasasına gidince fare kaçar (parayı da toplamış olur). |
| 🗑️ Çöp | Rakibin servis tezgâhını kapatır; bütün müşteriler sana gelir. | Sahibi tezgâhında 2 sn durup temizler. |
| 🧲 Mıknatıs | Sonraki 8 müşteri senin kasana gelir. | Yok, sadece süreyle biter. |
| 👟 Hız | 10 sn boyunca 1,5 kat hızlı koşarsın. | Kendine güçlendirme |
| 💰 ×2 Fiyat | 10 sn boyunca satışların iki kat para getirir. | Kendine güçlendirme |

**Halat:** Üstteki çubuk kimin ne kadar önde olduğunu gösterir. Maç her zaman 3 dakika sürer.

## Neden v0.2?

v0.1.2'yi oynayınca iki sorun çıktı: sabotaja zaman kalmıyordu, ve sabotaj bir insanın değil botun yapacağı bir hesap gibi hissettiriyordu (rakip nerede, kaç et var, gidip dönsem ne kaybederim?). Bu yüzden:

- Döngü beş duraktan üçe indi (bant).
- Rakibin istasyonunda durup çalma ve taşınan çöp kalktı. İkisi de Ayarlar'da "Eski" olarak kapalı duruyor.
- Yerine tek bir içgüdüsel karar geldi: ortaya düşen kutu.

Ölçüm sırasında bir ekonomi dersi de çıktı. Et kıtken, müşteri çeken etkiler (mıknatıs, tezgâh kapatma) rakibin satışını sadece geciktiriyordu; eti tezgâhta bekleyip sonra yine satılıyordu. Bu yüzden et yeniden bollaştı (1,25 sn). Artık asıl sınır müşteri sayısı ve çekilen her müşteri rakip için gerçek kayıp. Fare de eti değil parayı hedef alıyor.

Bot-vs-bot simülasyonu (80 maç, bot tepkisi 1 sn):

| Ölçüt | Değer |
|---|---|
| Oyuncu başı satış | 75 |
| Ortalama skor farkı | $81 |
| Yakın maç (fark %20'den az) | %88 |
| Daha çok kutu alanın kazanma oranı | %70 |

Kutular önemli ama tek başına maçı belirlemiyor.

### v0.2.1 notu: tezgâh kapasitesi

"İşletmecilik önemini korusun" diye tezgâh kapasitesini 8'e indirmeyi denedik. Ölçüm bunun tersini gösterdi: müşteri daha dolu tezgâha gittiği için dükkânı bırakmanın bedeli zaten bu stok yarışından geliyor. Kapasite 8 olunca iki taraf da hep 8'de eşitlenip bu bedel kayboluyor. Bu yüzden varsayılan 40 oldu.

Dükkânı bıraktığın süre içinde satış (sen / rakip, 30'ar deneme):

| Bırakma süresi | Tezgâh 8 | Tezgâh 40 |
|---|---|---|
| 8 sn | 2,5 / 3,5 | 3,1 / 3,6 |
| 15 sn | 5,8 / 5,8 | 3,1 / 7,5 |
| 30 sn | 10,8 / 11,4 | 3,9 / 18,6 |

Kutuya kısa bir koşu neredeyse bedava, uzun süre gitmek ağır kayıp. 5 çeşit kutuyla 40 bot maçında ortalama fark $156, yakın maç %60, daha çok kutu alan %72 kazanıyor.

## Ayarlar (denge testi)

Menüdeki **Ayarlar** bölümünden kutu aralığını, fare ve mıknatıs gücünü, bant süresini, et üretimini, hızı, kapasiteyi, müşteri sıklığını, fiyatı ve bot davranışını değiştirebilirsin. Kısa döngü, kutu, çöpün nereye düştüğü, farenin neyi kemirdiği gibi kurallar ve eski çalma/çöp mekanikleri açılıp kapatılabilir. Değerler tarayıcıda saklanır.

Hızlı denge testi için tarayıcı konsoluna (F12) şunu yaz:

```js
MW.simulate(20)
```

20 bot-vs-bot maçını anında oynatır ve skorları ve istatistikleri döner. `MW.step(saniye)` oyunu elle ileri sarar.

Ayarlar `mw_params_v7` anahtarıyla saklanır; önceki sürümlerde kaydettiğin ayarlar yeni varsayılanlara sıfırlanır.

## Sonraki adımlar

- Online 1v1: WebRTC (ör. PeerJS) ile oda kodu üzerinden ücretsiz bağlantı
- Maç içi olaylar (VIP müşteri, mutlu saat, ızgara alevi)
- Maç içi geliştirmeler (2. ızgara, dolap kilidi; zemindeki satın alma alanları)
- Gerçek motor (Unity) ile 2.5D görseller
