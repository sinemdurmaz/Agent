# 🤖 Kurumsal İK (HR) ReAct Ajanı

> **“Sadece cevap vermez; düşünür, planlar ve hesaplar.”**

Bu proje, **NLP (Doğal Dil İşleme)** dersi bitirme projesi kapsamında geliştirilmiş,  
**ReAct (Reasoning + Acting)** mimarisine sahip otonom bir **İnsan Kaynakları (HR) Asistanı**dır.

Klasik RAG sistemlerinden farklı olarak bu ajan:
- Soruları **alt problemlere böler**
- **Matematiksel muhakeme** yapar
- **İstisnai durumları (Edge Cases)** yakalayabilir
- LLM’i yalnızca metin üretici değil, **karar verici (Orchestrator)** olarak kullanır

---

## 🚀 Proje Özellikleri

### 🧠 ReAct Mimarisi
Ajan, problemleri şu döngüyle çözer:

**Thought → Action → Observation → Final Answer**

Bu yapı sayesinde:
- Ne zaman **araç kullanacağına**
- Ne zaman **bilgi arayacağına**
- Ne zaman **hesaplama yapacağına**

kendisi karar verir.

---

### 🛠️ Tool Use (Araç Kullanımı)

Ajan, dış dünyayla etkileşime geçebilen araçlara sahiptir:

| Araç | Açıklama |
|----|----|
| `hr_policy_search` | Vektör tabanlı şirket politikası araması |
| `calculator` | Maaş, prim, harcırah vb. hesaplamalar |

---

### 📚 Agentic RAG
- Statik metin getirme yerine
- Politika metnini **yorumlayarak**
- Bağlama uygun şekilde yanıt üretir

---

### 🧮 Matematiksel Muhakeme
- Maaş
- Prim
- Harcırah
- Katsayı bazlı hesaplamalar

Metin içinden sayısal veriler ayıklanır ve **hesaplanır**.

---

### 🛡️ Edge Case Yönetimi
Ajan şu durumları algılar:
- Eksik bilgi
- Yetkisiz talepler
- Etik ihlaller
- Deneme süresi / koşullu kısıtlar

---

## 📂 Veri Seti (Knowledge Base)

Projede, gerçekçi bir kurumsal simülasyon için  
**50+ maddeden oluşan**, JSON formatında yapılandırılmış özel bir veri seti kullanılmıştır.

**Dosya:** `sirket_politikalari.json`

### Veri Seti Kapsamı

- 💰 Maaş, Prim ve ESOP politikaları  
- ✈️ Çoklu para birimli harcırahlar (USD, EUR, TL)  
- 🏠 Hibrit & uzaktan çalışma kuralları  
- 🎁 Etik ve hediye kabul limitleri  
- ⚖️ İzinler ve kıdem hakları  

---

## 🛠️ Kurulum ve Çalıştırma

### 1️⃣ Repoyu Klonlayın
```bash
git clone https://github.com/KULLANICI_ADINIZ/HR-ReAct-Agent.git
cd HR-ReAct-Agent
2️⃣ Gerekli Kütüphaneleri Yükleyin
bash
Kodu kopyala
pip install -r requirements.txt
3️⃣ API Anahtarını Tanımlayın
Bu proje Groq API üzerinden Llama-3-70B modelini kullanır.

Seçenek 1: main.py içinde
python
Kodu kopyala
api_key = "gsk_..."
Seçenek 2: Ortam değişkeni (önerilir)
bash
Kodu kopyala
export GROQ_API_KEY="gsk_..."
4️⃣ Ajanı Başlatın
bash
Kodu kopyala
python main.py
🧪 Test Senaryoları (Benchmarks)
Senaryo Tipi	Kullanıcı Sorusu	Ajanın Muhakeme Süreci
Karmaşık Matematik	“Yıllık 600k maaşım ve 5 notum var, primim ne kadar?”	Maaş/12 → Katsayı (2.5) → Hesaplama → 125.000 TL
Çoklu Para Birimi	“3 gün Paris, 2 gün Tokyo harcırahım nedir?”	3×130 EUR + 2×110 USD → 390 EUR + 220 USD
Koşullu İstisna	“İşe dün başladım, evden çalışabilir miyim?”	Deneme süresi kontrolü → Hayır
Etik Kontrolü	“Tedarikçiden 100$ hediye geldi, alabilir miyim?”	Limit (50$) < 100$ → Yasak

🏗️ Mimari Şeması
mermaid
Kodu kopyala
graph TD
    User[Kullanıcı Sorusu] --> Agent[Llama-3 ReAct Ajanı]
    Agent --> Thought[Düşünce]
    Thought --> Router{Araç Seçimi}

    Router -->|Politika| RAG[Vektör Veritabanı]
    Router -->|Hesaplama| Calc[Hesap Makinesi]

    RAG --> Agent
    Calc --> Agent

    Agent --> Final[Nihai Cevap]
