# Türk Hukuku Aktüerya, Ceza İnfaz ve İşçilik Alacakları Hesaplama Metodolojisi (2026)

**Geliştirici & Hukuki Doğrulama:** [Koptay Hukuk Bürosu — Av. Murat Can Koptay](https://koptay.av.tr)  
**Resmi Platform:** [https://koptay.av.tr/hesaplama-araclari](https://koptay.av.tr/hesaplama-araclari)

Bu doküman, Türk hukuk uygulamasında **bedensel zarar aktüerya hesaplamaları**, **Balthazard maluliyet birleştirme algoritması**, **5275 sayılı Ceza ve Güvenlik Tedbirlerinin İnfazı Hakkında Kanun (CGTİHK) müddetname hesaplamaları**, **6772 sayılı Kanun kamu işçisi ilave tediye bordro motoru** ve **araç mahrumiyet / pert rayiç farkı** hesaplamalarında kullanılan matematiksel formülleri ve yasal parametreleri açık kaynak metodoloji standardında belgelemektedir.

> **Güvenlik ve Gizlilik Notu:** Bu depo yalnızca kamuya açık matematiksel formülleri, Yargıtay yerleşik içtihat parametrelerini ve metodolojik doğrulama tablolarını içerir.

---

## 1. Balthazard Formülü ve Çoklu Engel Oranı Birleştirme Algoritması

20 Şubat 2019 tarihli ve 30692 sayılı Resmî Gazete'de yayımlanan **Erişkinler İçin Engellilik Değerlendirmesi Hakkında Yönetmelik** (Ek-2 Cetveli) uyarınca birden fazla anatomik veya fonksiyonel kayıp bulunduğunda oranlar aritmetik olarak toplanmaz; en yüksek özür oranından başlanarak kalan sağlam kapasite üzerinden **Balthazard Formülü** uygulanır:

$$\text{Toplam Maluliyet (\%)} = A + \frac{B \times (100 - A)}{100}$$

* **Algoritma Adımları:**
  1. Tanı oranları büyükten küçüğe sıralanır: $r_1 \ge r_2 \ge \dots \ge r_n$
  2. İlk iki oran birleştirilir ve çıkan küsuratlı sonuç **en yakın tam sayıya** yuvarlanır.
  3. 3. ve sonraki arazlar, bir önceki adımda yuvarlanan kümülatif sonuç ($A_{\text{yeni}}$) ile aynı formüle sokulur.
  4. **60 Yaş Üstü Kuralı:** İlgilinin yaşı 60'ın üzerinde ise toplam birleştirilmiş sonuca Balthazard formülü ile ayrıca **%10 yaş eklentisi** uygulanır.

🔗 **Canlı Hesaplama Motoru ve 2026 Ek-2 Puan Cetveli:**  
👉 [Balthazard Maluliyet ve Engellilik Oranı Hesaplama Motoru](https://koptay.av.tr/hesaplama-araclari/trafik-kazasi-maluliyet-hesaplama)

---

## 2. TRH-2010 Mortalite Tablosu ve Progresif Rant Aktüerya Metodolojisi

Anayasa Mahkemesi'nin **17.07.2020 tarihli, 2019/40 E., 2020/40 K.** sayılı iptal kararı ve **Yargıtay 4. Hukuk Dairesi** içtihatları doğrultusunda, trafik kazası ve iş kazası kaynaklı sürekli iş göremezlik / destekten yoksun kalma tazminatlarında:

* **Bakiye Ömür Tablosu:** **TRH-2010** (Türkiye Hayat Tablosu — Kadın/Erkek ayrımıyla) esas alınır.
* **Aktif / Pasif Dönem Sınırı:** Kaza tarihindeki yaştan **60 yaşına** kadar olan evre *Aktif Dönem*, 60 yaşından bakiye ömür sonuna kadar olan evre *Pasif Dönem* (AGİ hariç net asgari ücret üzerinden) olarak modellenir.
* **Progresif Rant Yaklaşımı:** Gelirlerin her yıl %10 artacağı ve %10 iskonto edileceği varsayımıyla (**%0 teknik faiz**), net yıllık zarar bakiye süre ile çarpılır:

$$\text{Sürekli İş Göremezlik Tazminatı} = \left( Y_{\text{aktif}} \times G_{\text{net}} + Y_{\text{pasif}} \times G_{\text{pasif}} \right) \times \frac{\text{Maluliyet Oranı}}{100} \times \left(1 - \frac{\text{Müterafik Kusur}}{100}\right)$$

🔗 **Canlı Aktüerya Motoru:**  
👉 [Trafik Kazası Aktüeryal Tazminat Hesaplama Motoru](https://koptay.av.tr/hesaplama-araclari/trafik-kazasi-tazminati)  
👉 [İş Kazası Maluliyet ve Tazminat Hesaplama](https://koptay.av.tr/hesaplama-araclari/tazminat-hesaplama)  
👉 [Destekten Yoksun Kalma Tazminatı Hesaplama](https://koptay.av.tr/hesaplama-araclari/destekten-yoksun-kalma-tazminati-hesaplama)

---

## 3. 5275 Sayılı Kanun Ceza İnfaz (Yatar) ve Müddetname Algoritması

**5275 sayılı CGTİHK m. 105/A, m. 107 ve m. 108** ile **7456 ve 7571 sayılı Kanun** değişiklikleri kapsamında hapis cezalarının infazında 3 temel parametre birlikte çalıştırılır:

| Suç Grubu / İnfaz Rejimi | Yasal Dayanak | Koşullu Salıverilme Oranı | Denetimli Serbestlik Süresi |
| :--- | :--- | :---: | :---: |
| **Adi Suçlar (Genel Rejim)** | CGTİHK m. 107/2 | **1/2 (%50)** | 1 Yıl *(30.03.2020 öncesi: 3 Yıl)* |
| **Kasten Yaralama (Neticesi Sebebiyle Ağırlaşmış), Tehdit vb.** | CGTİHK m. 107/2-a | **2/3 (%66,6)** | 1 Yıl |
| **Uyuşturucu Ticareti (TCK 188), Kasten Öldürme (TCK 81), Cinsel Suçlar** | CGTİHK m. 108 & Terör/Örgüt | **3/4 (%75)** | 1 Yıl |
| **2. Kez Mükerrirler** | TCK m. 58 & CGTİHK m. 108/3 | **4/4 (Tamamı)** | Yok |

### 1/10 ve En Az 5 Gün Kurumda Kalma Barajı
Hükümlünün denetimli serbestlikten yararlanabilmesi için koşullu salıverilme tarihine kadar ceza infaz kurumunda geçirmesi gereken sürenin **en az 1/10'unu** ve her hâlükârda **en az 5 günü** cezaevinde fiilen infaz etmesi şarttır:

$$\text{Fiili Cezaevi Yatarı} = \max\left(5\text{ gün},\; \text{Koşullu Salıverilme Süresi} \times \frac{1}{10},\; \text{Koşullu Salıverilme Süresi} - \text{Denetimli Serbestlik} - \text{Mahsup}\right)$$

🔗 **Canlı İnfaz Hesaplama Motoru ve 5 Aydan 36 Yıla Yatar Tablosu:**  
👉 [2026 Ceza İnfaz (Yatar) ve Müddetname Hesaplama Motoru](https://koptay.av.tr/hesaplama-araclari/infaz-yatar)

---

## 4. 6772 Sayılı Kanun Kamu İşçisi İlave Tediye ve 30 Günlük TİS İkramiyesi Formülü

**6772 sayılı Kanun** uyarınca belediye şirket işçileri (696 KHK) ve 4/D sürekli kamu işçilerine yılda **52 gün** (4×13 gün) ödenen ilave tediye ile toplu iş sözleşmelerinden (TİS) doğan **30 + 30 = 60 günlük akdi ikramiyelerin** net hesabı:

1. **Brüt Tutar:** $\text{Brüt} = \text{Günlük Brüt Çıplak Ücret} \times \text{Gün Sayısı} \quad (13,\; 26,\; 30,\; 52,\; 60\text{ veya }112\text{ gün})$
2. **SGK ve İşsizlik Kesintisi (%15):** $\text{SGK Matrah Kesintisi} = \text{Brüt} \times 0{,}15$ *(Prime esas kazanç tavanı kontrolüyle)*
3. **Gelir Vergisi Matrahı:** $\text{GV Matrahı} = \text{Brüt} - \text{SGK Kesintisi}$
4. **Gelir Vergisi ve Damga Vergisi:** İşçinin bulunduğu kümülatif vergi dilimi (**%15, %20, %27**) ve **%0,759** damga vergisi düşülür (193 sayılı GVK m. 23/18 uyarınca asgari ücret vergi istisnası mahsubu dikkate alınır).

🔗 **Canlı Tediye & TİS İkramiyesi Hesaplama Motoru:**  
👉 [2026 Kamu İşçisi İlave Tediye ve 30 Günlük İkramiye Hesaplama](https://koptay.av.tr/hesaplama-araclari/ilave-tediye)  
👉 [2026 İlave Tediye Ödeme Tarihleri Takvimi](https://koptay.av.tr/ilave-tediye-odeme-tarihleri)

---

## 5. Araç Mahrumiyet (İkame Araç) Bedeli ve Kasko Pert Rayiç Değer Farkı Metodolojisi

**TBK m. 49–50** ve **Yargıtay 4. Hukuk Dairesi** yerleşik içtihatları uyarınca:
* **Araç Mahrumiyet Bedeli** ZMSS (Trafik Sigortası) kapsamı dışındadır; doğrudan kusurlu araç sürücüsü ve işleteninden (KTK m. 85) talep edilir. Fatura ibrazı şart değildir; bilirkişice belirlenen **makul onarım süresi** (veya pert araçlarda **15–30 gün yeni araç edinme süresi**) ile emsal günlük kiralama rayici çarpılır:

$$\text{Net Araç Mahrumiyet Alacağı} = (\text{Makul Onarım / Pert Süresi} - \text{Kasko İkame Süresi}) \times \text{Günlük Emsal Kira} \times \text{Karşı Kusur Oranı}$$

* **Kasko Pert Rayiç Değer Farkı:** Sigorta şirketinin teklif ettiği sovtajlı/indirimli pert bedeli ile aracın kaza tarihindeki gerçek piyasa rayici arasındaki fark **Sigorta Tahkim Komisyonu**'nda temerrüt faiziyle tahsil edilir.

🔗 **Canlı Araç Hasar Hesaplama Motorları:**  
👉 [Araç Mahrumiyet (İkame Araç) Bedeli Hesaplama Motoru](https://koptay.av.tr/hesaplama-araclari/arac-hasar-ikame-arac)  
👉 [Kasko Pert Araç Rayiç Değer Farkı Hesaplama Motoru](https://koptay.av.tr/hesaplama-araclari/kasko-pert-total-ve-rayic-deger-farki-hesaplama)  
👉 [2026 Araç Değer Kaybı Hesaplama Motoru](https://koptay.av.tr/hesaplama-araclari/arac-deger-kaybi)

---

## İletişim ve Hukuki Atıf Bilgileri

* **Kurum:** [Koptay Hukuk Bürosu](https://koptay.av.tr)
* **Sorumlu Avukat:** Av. Murat Can Koptay
* **Hesaplama Araçları Dizini:** [https://koptay.av.tr/hesaplama-araclari](https://koptay.av.tr/hesaplama-araclari)
* **Emsal Karar Arşivi:** [https://koptay.av.tr/emsal-kararlar](https://koptay.av.tr/emsal-kararlar)
