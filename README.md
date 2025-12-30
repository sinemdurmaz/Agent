#  İş Hukuku Danışmanı ReAct Ajanı

> **“Kanunları sadece okumaz; yorumlar, hesaplar ve danışmanlık verir.”**

Bu proje, **4857 Sayılı İş Kanunu** kapsamında çalışanların ve işverenlerin hukuki sorularını yanıtlamak üzere geliştirilmiş, **ReAct (Reasoning + Acting)** mimarisine sahip otonom bir yapay zeka asistanıdır.

Klasik arama motorlarından farklı olarak bu ajan; kanun maddelerini **yorumlayarak** bağlama oturtur, kıdem tazminatı veya fazla mesai gibi konularda **matematiksel hesaplamalar** yapar ve hukuki **istisnaları** (deneme süresi, haklı fesih vb.) dikkate alır.

---

##  Proje Özellikleri

###  ReAct Mimarisi (Düşün-Hareket Et)
Ajan, karmaşık hukuki problemleri şu döngüyle çözer:
`Thought` (Düşünce) → `Action` (Eylem) → `Observation` (Gözlem) → `Final Answer` (Cevap)

Bu yapı sayesinde ajan; bir soruyu cevaplamak için önce kanuna bakması gerektiğini, ardından bulduğu oranı kullanarak hesaplama yapması gerektiğini **kendi planlar**.

###  Araçlar (Tools)
Ajan, hukuki süreçleri yönetmek için iki temel araca sahiptir:

| Araç | Açıklama |
| :--- | :--- |
| **`kanun_ara`** | 4857 Sayılı İş Kanunu PDF'i üzerinde vektör tabanlı semantik arama yapar ve ilgili maddeyi getirir. |
| **`calculator`** | Kıdem tazminatı, ihbar süresi, fazla mesai ücreti gibi hukuki matematik işlemlerini yapar. |

###  Agentic RAG
* Statik metin getirme yerine,
* Kanun maddesini (Örn: Madde 41 - Fazla Çalışma) analiz eder,
* Kullanıcının özel durumuna (Örn: "Maaşım 20.000 TL") göre yanıtı özelleştirir.

###  Hukuki Matematik
Metin içinde geçen *"yüzde elli artırımlı ödenir"* veya *"her yıl için 30 günlük ücret"* gibi sözel ifadeleri sayısal verilere dönüştürerek hesaplar.

---

##  Bilgi Tabanı (Knowledge Base)

Proje, Türkiye Cumhuriyeti'nin temel çalışma yasası olan **4857 Sayılı İş Kanunu**'nun tam metnini kullanır.

* **Kaynak:** `document.pdf` (İş Kanunu Tam Metni)
* **İşleme:** PDF verisi, `SimpleRAG` motoru ile "MADDE" bazlı parçalara (chunk) ayrılarak vektör veritabanına işlenmiştir.

---

##  Çalışma Mantığı



```mermaid
graph TD
    User[Kullanıcı: 'Fazla mesai ücretim ne kadar?'] --> Agent[Llama-3 Hukuk Ajanı]
    Agent --> Thought[Düşünce: Oranı kanundan bulmalıyım]
    Thought --> Router{Araç Seçimi}

    Router -->|Ara| Tool1[kanun_ara: 'Fazla çalışma ücreti oranı']
    Tool1 --> Obs1[Gözlem: 'Saatlik ücret %50 artırılır']
    
    Obs1 --> Agent
    Agent --> Thought2[Düşünce: Şimdi hesaplamalıyım]
    Thought2 --> Router
    
    Router -->|Hesapla| Tool2[calculator: 'Saatlik Ücret * 1.5']
    Tool2 --> Obs2[Gözlem: Sonuç]

    Obs2 --> Agent
    Agent --> Final[Cevap: 'Saatlik fazla mesai ücretiniz X TL'dir.']

 Kurulum ve Çalıştırma
1️⃣ Gereksinimler
Python 3.8+

Groq API Anahtarı (Llama-3 Modeli için)

document.pdf (İş Kanunu dosyası proje dizininde olmalıdır)

2️⃣ Kurulum
Bash

git clone [https://github.com/KULLANICI_ADINIZ/Is-Hukuku-Ajan.git](https://github.com/KULLANICI_ADINIZ/Is-Hukuku-Ajan.git)
cd Is-Hukuku-Ajan
pip install -r requirements.txt
3️⃣ API Anahtarı
Projenin çalışması için Groq API anahtarınızı tanımlayın:

Bash

export GROQ_API_KEY="gsk_..."
4️⃣ Çalıştırma
Bash

python ik_agen.py

 Benchmark ve Test Senaryoları
Ajanın hukuki yetkinliğini ölçmek için kullanılan 3 temel kategori ve örnekler:
Kategori,Soru Örneği,Ajanın Çözüm Yolu
Temel Hukuk Bilgisi,"""Haftalık çalışma süresi yasal olarak en çok kaç saattir?""",kanun_ara → Madde 63'ü bul → 45 Saat
Matematiksel Hesap,"""Brüt 20.000 TL maaşla 10 saat fazla mesai yaptım, ücreti ne kadar?""",kanun_ara (%50 oran) → calculator (20000/225 * 1.5 * 10) → ~1.333 TL
Mantıksal Çıkarım,"""Kendi isteğimle istifa edersem kıdem tazminatı alabilir miyim?""",kanun_ara (Kıdem şartları) → İstifa durumunu analiz et → Hayır (Haklı neden yoksa)

 Lisans
Bu proje eğitim amaçlı geliştirilmiştir ve hukuki tavsiye niteliği taşımaz. Nihai kararlar için bir hukukçuya danışılmalıdır. MIT License altında lisanslanmıştır.
