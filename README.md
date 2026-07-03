# 📊 E-Ticarət Platformasında Hipotezlərin Prioritetləşdirilməsi və A/B Test Analizi

Bu layihə, e-ticarət platformasında biznes göstəricilərini (gəlir, konversiya, istifadəçi aktivliyi) artırmaq məqsədilə irəli sürülmüş müxtəlif məhsul hipotezlərinin analitik çərçivədə qiymətləndirilməsini və keçirilmiş **A/B testinin nəticələrinin statistik olaraq analiz edilməsi** prosesini əhatə edir.

Layihə iki əsas mərhələdən ibarətdir:
1. **Framework-ə əsaslanan prioritetləşdirmə** (ICE və RICE metodologiyası).
2. **A/B testinin nəticələrinin analizi** (Kumulyativ metriklər, anomalilərin təmizlənməsi və qeyri-parametrik statistik testlər).

---

## 📂 Data Struktur və Metadata

Layihədə üç müxtəlif kontekstli verilənlər bazasından (Dataframe) istifadə olunur:

### 1. `hypotheses_us.csv` (Hipotezlər Siyahısı)
Biznesin inkişafı üçün təklif olunmuş 9 unikal hipotezi və onların ekspert qiymətləndirmə ballarını (1-10 şkalası üzrə) saxlayır.
* **Hypothesis** (*Object*) — Hipotezin qısa təsviri.
* **Reach** (*Int64*) — Dəyişikliyin təsir edəcəyi istifadəçilərin nisbi sayı.
* **Impact** (*Int64*) — Dəyişikliyin istifadəçi təcrübəsinə və konversiyaya təsir gücü.
* **Confidence** (*Int64*) — Ekspertlərin verilən balların dəqiqliyinə olan əminliyi.
* **Effort** (*Int64*) — Hipotezin tətbiqi üçün tələb olunan resurs, vaxt və xərc.

### 2. `orders_us.csv` (Sifarişlər)
Test müddətində qeydə alınmış 1,197 transaksiyanın detalları.
* **transactionId** (*Int64*) — Hər bir sifarişin unikal identifikasiya nömrəsi.
* **visitorId** (*Int64*) — Sifarişi edən müştərinin unikal ID-si.
* **date** (*Object*) — Sifarişin həyata keçirildiyi tarix.
* **revenue** (*Float64*) — Sifarişdən əldə olunan maliyyə gəliri.
* **group** (*Object*) — İstifadəçinin daxil olduğu test qrupu (`A` və ya `B`).

### 3. `visits_us.csv` (Ziyarətlər)
Qruplar üzrə gündəlik platforma daxilolmalarının (trafikin) ümumi sayı (62 sətir).
* **date** (*Object*) — Müşahidə tarixi.
* **group** (*Object*) — Test qrupu (`A` və ya `B`).
* **visits** (*Int64*) — Həmin tarixdə müvafiq qrup üzrə ümumi sessiya (ziyarət) sayı.

--- DATA SETLƏR
Datasetləri bu linkdən əldə edə bilərsiniz: https://drive.google.com/drive/folders/1rZAy9bM67yR9-8DR4llfhkMnr8gl7ICA?usp=sharing

## 🛠️ Texnoloji Alətlər (Tech Stack)

Layihə **Python 3** mühitində təməl data elmi kitabxanalarından istifadə edilərək hazırlanmışdır:
* **Pandas** — Məlumatların strukturlaşdırılması, aqreqasiyası və filtrasiyası.
* **NumPy** — Vektorlaşdırılmış riyazi hesablamalar və massiv manipulyasiyası.
* **Matplotlib & Seaborn** — Statistik qrafiklərin, kumulyativ trendlərin və qutu qrafiklərinin (Boxplot) vizuallaşdırılması.
* **SciPy (`scipy.stats`)** — Paylanmaların analizi və fərqlərin statistik əhəmiyyətliliyinin (P-value) yoxlanılması.

---

## 📈 Metodologiya və Analitik Addımlar

### 1. Hipotezlərin Prioritetləşdirilməsi
Məhdud resurslarla maksimum gəlir əldə etmək üçün hipotezlər iki fərqli metodla sıralanır:
* **ICE Skorinq:** Məhsul daxili təsirə fokuslanır.
  $$\text{ICE} = \frac{\text{Impact} \times \text{Confidence}}{\text{Effort}}$$
* **RICE Skorinq:** İstifadəçi kütləsinin əhatə dairəsini də hesaba qatır.
  $$\text{RICE} = \frac{\text{Reach} \times \text{Impact} \times \text{Confidence}}{\text{Effort}}$$

### 2. Data Preprocessing & Sanitization
* Tarix sütunları manipulyasiya üçün `datetime64` tipinə çevrilir.
* **Kritik Yoxlanış:** Eyni anda həm A, həm də B qrupunda alış-veriş edən istifadəçilər müəyyən edilir və testin doğruluğunu qorumaq üçün orders datasından təmizlənir.

### 3. Kumulyativ Metriklərin Analizi
Data zaman oxu boyunca yığılaraq (cumulative) hesablanır:
* Günlər üzrə kumulyativ gəlir qrafiki (Qrupların gəlir trendi).
* Günlər üzrə kumulyativ orta sifariş məbləği (Average Ticket).
* Qruplar arası nisbi konversiya qrafiki.

### 4. Anomalilərin (Outliers) Təmizlənməsi
* İstifadəçi başına düşən sifariş sayının və sifariş məbləğlərinin paylanması analiz edilir.
* **95-ci və 99-cu persentillər** hesablanaraq anomalilərin sərhədi (threshold) təyin olunur və ekstremal böyük alış-verişlər filtering vasitəsilə kənarlaşdırılır.

### 5. Hipotez Testləri (Statistical Significance)
Qruplar arasında konversiya və orta sifariş məbləğində fərq olub-olmaması yoxlanılır. Data normal paylanmaya tabe olmadığı üçün qeyri-parametrik **Mann-Whitney U testi** tətbiq edilir:
* Xam data (Raw Data) üzərində statistik analiz.
* Təmizlənmiş data (Filtered Data) üzərində statistik analiz.
* P-value və Alpha ($\alpha = 0.05$) səviyyələrinin müqayisəsi.

---

## 📌 Layihənin Strukturlaşdırılmış İcra Sxemi

```text
├── [1] Məlumatların Yüklənməsi & Google Drive İnteqrasiyası
├── [2] İlkin Data İnspeksiyası (.info(), .head(), tiplərin yoxlanılması)
├── [3] ICE və RICE Prioritetləşdirmə Analizi
├── [4] Data Təmizləmə (Tiplərin korreksiyası, çarpaz istifadəçilərin silinməsi)
├── [5] Kumulyativ Qrafiklərin Qurulması (Gəlir, Orta Çek, Konversiya)
├── [6] Persentil Analizi & Outlier Filtrasiyası
├── [7] Mann-Whitney U Statistik Testləri
└── [8] Biznes Qərarının Verilməsi (Testin Nəticəsi)
