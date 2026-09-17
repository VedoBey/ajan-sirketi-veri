# Ajan Şirketi — Veri Deposu

Bu depo, BaturOS vault'undaki "Ajan Şirketi" otomasyonunun (Kurucu, Fink, Mani,
Denetçi, Yönetici) durum verilerini tutar.

**Neden burada, Şirket Panosu Artifact'inin veritabanında değil:** Zamanlanmış
cloud rutinler (RemoteTrigger/CCR) Artifact veritabanına yazarken onaylanamayan
bir izin isteğine takılıyor (bkz. BaturOS vault hafızası:
`ajan-sirketi-cron-yazma-arizasi.md`). Dosya/git yazımı ise onaysız çalışıyor.
Bu yüzden ajanlar buraya yazıyor; Şirket Panosu'nun canlı görünümü ise bu
depodan **periyodik olarak elle senkronize edilerek** güncelleniyor (depo
private olduğu için pano tarayıcıdan doğrudan okuyamıyor).

## Şema

Her klasör bir "koleksiyon", içindeki her `.json` dosyası bir "doküman"
(dosya adı = doc_id). Alan adları ve anlamları Şirket Panosu'nun okuduğu
Artifact koleksiyonlarıyla birebir aynı:

- `calisanlar/<id>.json` — `{ durum, sonOzet, sonCalisma, sonrakiCalisma, dikkatSayisi }`
- `fikirler/<YYYY-MM-DD>.json` — Fink'in günlük yan gelir fikirleri
- `brifing/<YYYY-MM-DD>.json` — Kurucu'nun günlük gündem brifingi
- `finans_ozet/<YYYY-MM-DD>.json` — Mani'nin günlük finans özeti (sadece özet, ham işlem yok)
- `denetim/<tarih>.json` — Yönetici'nin haftalık içerik-kalite denetimi
- `denetim_guvenlik/<tarih>.json` — Denetçi'nin haftalık güvenlik/uyum denetimi
- `kuyruk/<id>.json` — Fink/Mani/HypeClip Koordinatörü için açık yönlendirmeler
- `ajan_onerileri/<id>.json` — yeni ajan önerileri (Öneri → Denetçi → Enes onayı)
- `hypeclip_ozet/<tarih>.json` — HypeClip Media birim özeti

Her ajan kendi koşusunda ilgili klasöre commit+push yapar. Senkron talep
edildiğinde bu dosyalar okunup Şirket Panosu'nun Artifact veritabanına
aktarılır.
