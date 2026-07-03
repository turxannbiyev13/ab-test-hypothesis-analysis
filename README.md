# 📊 A/B Test Hipotez Analizi (A/B Test Hypothesis Analysis)

Bu layihədə, bir e-ticarət və ya veb-sayt platformasında istifadəçi təcrübəsini təkmilləşdirmək məqsədilə keçirilən A/B testinin nəticələri statistik metodlarla analiz edilir. Layihənin əsas məqsədi, yeni dizaynın (və ya xüsusiyyətin) konversiya göstəricilərinə (Conversion Rate) statistik olaraq əhəmiyyətli dərəcədə təsir edib-etmədiyini müəyyən etməkdir.

## 🚀 Layihənin Məqsədi
Biznes nöqteyi-nəzərindən mövcud veb-səhifənin (Control qrupu) performansı ilə yeni təklif olunan dizaynın (Treatment qrupu) performansı müqayisə edilir. Statistik testlər vasitəsilə təsadüfi uğurlarla real effekti bir-birindən ayıraraq, yeni dizayna keçid barədə düzgün qərar verilməsi hədəflənir.

## 📈 Metodologiya və Addımlar
Analiz prosesi aşağıdakı ardıcıllıqla icra olunmuşdur:

1. **Eksperimentin Dizaynı (Experimental Design):**
   - Hipotezlərin qurulması ($H_0$ və $H_a$).
   - Əhəmiyyətlilik dərəcəsinin ($\alpha = 0.05$) təyini.
2. **Məlumatların Hazırlanması (Data Cleaning & Prep):**
   - Dublikat və uyğunsuz (Control qrupuna yeni səhifə, Treatment qrupuna köhnə səhifə düşən) dataların təmizlənməsi.
   - Seçmə ölçüsünün (Sample size) adekvatlığının yoxlanılması.
3. **Vizuallaşdırma və İlkin Analiz (EDA):**
   - Qruplar üzrə konversiya nisbətlərinin qrafik təsviri.
4. **Hipotez Testi (Hypothesis Testing):**
   - İki proporsiyanın müqayisəsi üçün **Z-test** tətbiqi.
   - P-value və Kritik Qiymətlərin hesablaqlanması.
5. **Nəticə və Biznes Qərarı (Conclusion):**
   - Statistik tapıntıların biznes dilinə tərcüməsi.

## 🛠️ İstifadə Olunan Texnologiyalar
Layihə **Python** proqramlaşdırma dilində, Jupyter Notebook mühitində yazılmışdır. Əsas kitabxanalar:
* `pandas` — Məlumatların manipulyasiyası və təmizlənməsi
* `numpy` — Riyazi və statistik hesablamalar
* `matplotlib` & `seaborn` — Məlumatların vizuallaşdırılması
* `statsmodels` — Statistik testlərin (Z-test) icrası

## 📊 Hipotezlər
* **Sıfır Hipotez ($H_0$):** Köhnə və yeni dizaynın konversiya nisbətləri arasında heç bir fərq yoxdur ($p_{new} = p_{old}$ və ya $p_{new} - p_{old} = 0$).
* **Alternativ Hipotez ($H_a$):** Yeni dizaynın konversiya nisbəti köhnədən fərqlidir/yüksəkdir ($p_{new} \neq p_{old}$ və ya $p_{new} > p_{old}$).
