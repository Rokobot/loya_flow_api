# Vibe Mobile Repo Usage Rules
<img width="10500" height="4620" alt="Frame 18" src="https://github.com/user-attachments/assets/1cd16bcf-0711-48f1-994a-927f98f69cc9" />


Bu doküman Vibe mobil reposunda branch isimlendirme ve kullanım kurallarını tanımlar. Ekipteki herkes bu standarda uyar. Amaç, repoya bakan herkesin hangi branch'in ne işe yaradığını **tek bakışta** anlayabilmesidir.

---

## Genel Kural

Branch ismine **kişi adı yazılmaz.** Branch'i push eden kişinin ismi Git ve GitHub'da zaten otomatik görünür, bu yüzden isme gerek yoktur. Branch ismi "kim" sorusunu değil, **"bu branch ne iş yapıyor"** sorusunu yanıtlar.

**Format:**

```
tip/kisa-aciklama
```

- Hep **küçük harf**
- Kelimeler arası **tire** (`-`)
- Açıklama **2-4 kelime**, kısa ve net olsun

---

## Branch Tipleri ve Anlamları

### `feature/` — Yeni Özellik

Uygulamaya yeni bir işlevsellik eklendiğinde kullanılır. Yeni ekran, yeni bir sistem, yeni bir akış vb.

`main` üzerinden açılır, iş bitince PR ile tekrar `main`'e merge edilir.

```
feature/live-gift-lottie
feature/media-editor-screen
feature/new-profile-redesign
```

### `fix/` — Normal Bug Düzeltme

Acil olmayan, sıradan hata düzeltmeleri için kullanılır. Production'ı yakan bir durum değildir; normal akışta `main` üzerinden gider.

`main` üzerinden açılır, PR ile `main`'e döner.

```
fix/player-next-track-bug
fix/filter-slider-behavior
fix/bottomsheet-layout
```

### `hotfix/` — Production'daki Acil Bug

Yayında olan sürümde kullanıcıyı etkileyen, beklemeye tahammülü olmayan kritik bir hata için kullanılır.

Normal akıştan **farklıdır:** `main`'den değil, **yayında olan sürümün tag'inden** açılır. Düzeltme hem store'a gönderilir hem de `main`'e geri merge edilir ki düzeltme kaybolmasın.

```
hotfix/iap-trial-display
hotfix/livescreen-crash
hotfix/payment-verification
```

### `release/` — Store'a Gönderilecek Dondurulmuş Sürüm

Store'a (Play Store / App Store) gönderilecek sürümün dondurulduğu branch'tir. Bu branch açıldıktan sonra içine **yeni özellik girmez**, sadece o sürüme ait son bug fix'ler girer.

Store review'da beklerken `main`'de bir sonraki sürümün işine devam edilebilmesini sağlar. Sürüm yayınlanınca commit `tag` ile işaretlenir (`v1.4.0`).

```
release/1.4.0
release/1.5.0
```

### `chore/` — Kod Dışı İşler

Uygulama davranışını değiştirmeyen bakım işleri için kullanılır. Paket güncelleme, config değişikliği, Flutter sürüm yükseltme, dosya düzenleme vb.

`main` üzerinden açılır, PR ile `main`'e döner.

```
chore/flutter-3-27-upgrade
chore/update-dependencies
chore/ci-config
```

---

## Özet Tablo

| Tip | Ne için | Nereden açılır | Nereye merge olur |
|-----|---------|----------------|-------------------|
| `feature/` | Yeni özellik | `main` | `main` |
| `fix/` | Normal bug düzeltme | `main` | `main` |
| `hotfix/` | Production acil bug | Yayındaki tag | `main` + store |
| `release/` | Sürüm dondurma | `main` | tag'lenir |
| `chore/` | Kod dışı bakım işleri | `main` | `main` |

---

## Çalışma Akışı (Özet)

1. `main` her zaman stabil ve deploy edilebilir durumdadır.
2. Yeni iş için uygun tip ile branch açılır (`feature/...`, `fix/...` vb.).
3. İş bitince **Pull Request (PR)** açılır.
4. PR review'dan geçer ve CI yeşil olursa `main`'e merge edilir.
5. Branch'ler kısa ömürlü tutulur — 2-3 günden fazla bekletilmez ki conflict birikmesin.

---

## Hatırlatmalar

- `main`'e **doğrudan push yapılmaz**, her şey PR'dan geçer.
- Branch ne kadar küçük ve odaklı olursa review o kadar kolay olur.
- Branch tipi listesi sabittir — bu 5 tipin dışına çıkılmaz, kalabalık yaratmamak için.
