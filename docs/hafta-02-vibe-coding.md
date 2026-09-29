# Hafta 2 · Canlı vibe coding senaryosu ve istemler

Öğretim üyesi notu. Yansıda gösterilecek istemler sitede de var (Hafta 2 → "Derste kullanılan istem"); burası sunum akışı ve yedek planı içerir.

## Hazırlık (ders öncesi, 10 dk)
- Claude Code masaüstünü aç; `Belgeler/yz-saglik-hafta02` gibi **boş** bir klasörde başlat. Ajan yalnızca bu klasöre erişsin.
- Yansı yazı boyutunu büyüt (Cmd/Ctrl + "+" üç kez). Tarayıcıda hazır yedek sürüm açık bir sekmede dursun: `https://drferhatu.github.io/yapay-zeka-ve-saglik/araclar/klinik-hesaplayici`
- İnternet yoksa: yedek sürümü aç, "bu, aynı istemle önceki gece üretildi" de; istemi yansıda okut, koda geç. Akış bozulmaz.

## Ana istem (yansıda okunacak, 1. adım)

```
Tıp öğrencileri için Türkçe, tek dosyalık (HTML+CSS+JS, dış bağımlılık yok) bir "Klinik Hesaplayıcı" yap.
Beş sekme: (1) VKİ ve bel/boy oranı, (2) eGFR CKD-EPI 2021, (3) CHA2DS2-VASc, (4) Wells DVT, (5) mg/kg doz.
Her sekmede: girdi formu, "Hesapla" düğmesi, sonuç kutusu (değer + sınıf/evre + 2-3 maddelik klinik yorum ve sınırlılık), altında kaynak listesi (yazar, dergi, yıl, DOI bağlantısı).
Girdi hatalarını yakala (boy santimetre yerine metre girilirse uyar; kreatinin µmol/L girilirse uyar).
Kod okunabilir olsun: her fonksiyonun üstüne bir satır Türkçe açıklama, klinik eşikler if bloklarında açıkça görünsün, sihirli sayılar yorumlanmış olsun.
Tasarım sade ve mobil uyumlu; telefonda tek elle kullanılabilsin.
Formül ve eşik ayrıntıları aşağıda. Emin olmadığın bir eşik varsa uydurma, "doğrulanmalı" diye işaretle.
```

Ardından formül ayrıntıları yapıştırılır (aşağıda). İki mesaj hâlinde vermek öğrencinin "bağlam vermek" kavramını görmesi için iyidir.

## Formül ve eşik ayrıntıları (2. adım, istemin devamı)

```
1) VKİ = kilo(kg) / boy(m)². WHO: <18,5 zayıf; 18,5–24,9 normal; 25–29,9 fazla kilolu; 30–34,9 obez I; 35–39,9 obez II; ≥40 obez III.
   Bel/boy oranı = bel(cm)/boy(cm). <0,40 düşük; 0,40–0,49 sağlıklı; 0,50–0,59 artmış risk; ≥0,60 yüksek risk (Ashwell & Hsieh 2005; Browning 2010).
   Bel çevresi: kadın ≥80 artmış / ≥88 yüksek; erkek ≥94 / ≥102 (WHO 2008, IDF 2006).
2) eGFR CKD-EPI 2021 (Inker, NEJM 2021): 142 × min(Scr/κ,1)^α × max(Scr/κ,1)^-1,200 × 0,9938^yaş × (kadınsa 1,012).
   Kadın κ=0,7 α=-0,241; erkek κ=0,9 α=-0,302. Scr mg/dL. KDIGO evreleri: G1 ≥90, G2 60–89, G3a 45–59, G3b 30–44, G4 15–29, G5 <15.
3) CHA2DS2-VASc (Lip, Chest 2010): KKY 1, HT 1, yaş ≥75 2, DM 1, inme/GİA/TE 2, vasküler hastalık 1, yaş 65–74 1, kadın 1. Toplam 0–9.
   Yıllık inme riski (Friberg, EHJ 2012) %: 0:0,2 1:0,6 2:2,2 3:3,2 4:4,8 5:7,2 6:9,7 7:11,2 8:10,8 9:12,2.
   ESC 2024: cinsiyet puanı çıkarılmış CHA2DS2-VA; ≥2 antikoagülasyon önerilir, 1 düşünülmeli. İkisini de göster.
4) Wells DVT (Wells, Lancet 1997; NEJM 2003): aktif kanser 1; paralizi/parezi/alçı 1; ≥3 gün yatak veya 12 hafta içinde majör cerrahi 1;
   derin ven boyunca hassasiyet 1; tüm bacakta şişlik 1; baldır ≥3 cm fark 1; gode bırakan ödem 1; kollateral yüzeyel venler 1;
   önceki DVT 1; alternatif tanı en az DVT kadar olası −2. İki düzey: ≤1 olası değil, ≥2 olası. Üç düzey: 0 düşük (~%5), 1–2 orta (~%17), ≥3 yüksek (~%53).
5) mg/kg doz: günlük toplam = kilo × mg/kg/gün; doz başına = toplam / günlük doz sayısı; isteğe bağlı tavan doz (aşarsa tavanı uygula ve uyar);
   isteğe bağlı süspansiyon derişimi mg/5 mL → doz başına mL. Eğitim amaçlı olduğunu ve prospektüsle doğrulanması gerektiğini yaz.
```

## Bilerek hata senaryosu (3. adım, 5 dk)
1. Üretilen dosyayı aç, VKİ sekmesinde boya **1.74** yaz → uyarı çıkmalı ("boy santimetre olmalı"). Çıkmazsa daha iyi: hata canlı görülür.
2. Kodda `boy` değişkenini bulun; sınıfa sor: "Burada boy hangi birimde? Nerede metreye çevriliyor?"
3. Ajana yaz: `VKİ sekmesinde boy 3'ten küçük girildiğinde kullanıcı metre girmiş olabilir; otomatik olarak cm'ye çevir ve bunu sonuçta belirt.` Düzeltmeyi izleyin; diff'i yansıda gösterin.
4. İkinci hata (zaman kalırsa): eGFR'de kreatinin **97** girin (µmol/L). Uyarı çıkmalı; çıkarsa "modele bunu biz söylemiştik, istemin gücü" deyin.

## Kodu okuma turu (4. adım, 15 dk) — üç durak
- **Değişken ve tip:** `const kilo = Number(...)` satırı. Form alanı metin döner, `Number` sayıya çevirir. Soru: `Number` olmasa `"72" / "1.74"` ne olur?
- **Fonksiyon:** `function egfrCkdEpi2021(kreatinin, yas, cinsiyet)`. Parametreler formülün girdileri. Soru: "Cinsiyet formüle nerede giriyor?" (κ, α ve 1,012 çarpanı.)
- **Eşik/`if`:** `kdigoEvresi` içindeki `if (egfr >= 60)` zinciri. Soru: 59,6 hangi evre? Neden yuvarlamadan önce karşılaştırıyoruz?

## Colab (5. adım, 10 dk)
Hafta 2 defteri: aynı eGFR fonksiyonu Python'da; `def`, parametre, `return`, `if`. Sonda "deneyin": VKİ ve bel/boy.

## NotebookLM kapanışı (6. adım, 5 dk)
Kaynak olarak yükle: Hafta 1 ve 2 sayfaları (URL olarak), KDIGO 2024 özet PDF'i, ESC 2024 AF kılavuzu özeti. İste: "Bu kaynaklardan 5 soruluk çoktan seçmeli quiz yap, her sorunun altına kaynağını yaz." Ardından sesli özet düğmesini göster. Mesaj: "Ders çalışmanın yeni yolu; ama kaynağı siz yüklediniz, o yüzden uydurmuyor."

## Yedek plan özeti
| Sorun | Yapılacak |
|---|---|
| İnternet yok | Yedek sürüm + istemi yansıda okut + kod turu |
| Claude Code kota/hesap | ChatGPT web'de aynı istem; çıktıyı bir .html dosyasına yapıştırıp çift tıkla aç |
| Model 15 dk'da bitiremedi | Bekleme sırasında kodu okuma turuna yedek sürümle başla |
