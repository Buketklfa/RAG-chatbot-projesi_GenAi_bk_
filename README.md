# RAG-chatbot-projesi_GenAi_bk_

# 🧬 Khan Academy Türkçe Biyoloji RAG Chatbot

## 🚀 Projenin Amacı
Bu proje, **Retrieval Augmented Generation (RAG)** mimarisini kullanarak, Hugging Face'teki [ysdede/khanacademy-turkish](https://huggingface.co/datasets/ysdede/khanacademy-turkish) veri setinden filtrelenmiş biyoloji içeriğine dayalı, sadece biyoloji sorularını cevaplayan, Türkçe bir chatbot geliştirmeyi amaçlamaktadır. Amaç, büyük dil modelinin (LLM) dışarıdan sağlanan bilgiye dayanarak (hallucination'ı azaltarak) cevap vermesini sağlamaktır.

## 📚 Veri Seti Hakkında Bilgi
* **Kaynak:** `ysdede/khanacademy-turkish` (Hugging Face)
* **Kullanım:** Veri setinin `train` split'i kullanılmıştır.
* **Hazırlık Metodolojisi:** Veri seti, anahtar kelime filtrelemesi (`biyoloji`, `hücre`, `DNA`, `evrim` vb.) ile taranarak yalnızca Biyoloji konularını içeren kayıtlar seçilmiş ve RAG için LangChain `Document` formatına dönüştürülmüştür.

## 🛠️ Çözüm Mimarisi (RAG Bileşenleri)
| Bileşen | Kullanılan Teknoloji / Model | Amaç |
| :--- | :--- | :--- |
| **Knowledge Base (Bilgi Tabanı)** | `ysdede/khanacademy-turkish` (Filtrelenmiş) | Chatbot'un biyoloji alanındaki bilgi kaynağını oluşturur. |
| **Data Chunks (Veri Parçacıkları)** | `RecursiveCharacterTextSplitter` (chunk\_size=1000, overlap=200) | Uzun belgeleri LLM'in bağlam penceresine sığacak mantıksal parçalara böler. |
| **Embedding Model (Gömme Modeli)** | `sentence-transformers/distiluse-base-multilingual-cased-v2` | Metin ve sorguları, çok dilli yetenekli sayısal vektörlere dönüştürür. |
| **Vector DB (Vektör Veritabanı)** | `Chroma` | Vektörleri depolar ve sorgu vektörüne en yakın, en alakalı parçaları (k=3) çeker. |
| **LLM (Büyük Dil Modeli)** | Google Gemini 2.5 Flash | Çekilen bağlamı okuyarak ve **Prompt Template**'teki kurallara uyarak son cevabı üretir. |

## ⚙️ Çalıştırma Kılavuzu (GitHub/Colab)

Projenin başarıyla çalışması için bir **Gemini API Anahtarı** gereklidir.

### 1. Ortam Hazırlığı (Lokal veya Colab)

1.  **Dosyayı İndirin:** `biyoloji_rag_chatbot.py` ve `requirements.txt` dosyalarını indirin.
2.  **Sanal Ortam Kurulumu (Önerilir):**
    ```bash
    python -m venv venv
    source venv/bin/activate # Linux/macOS
    .\venv\Scripts\activate   # Windows
    ```
3.  **Gerekli Kütüphaneleri Kurun:**
    ```bash
    pip install -r requirements.txt
    ```

### 2. API Anahtarını Ayarlama

1.  Gemini API Anahtarınızı [Google AI Studio](https://aistudio.google.com/api-keys) üzerinden alın.
2.  Anahtarı ortam değişkeni olarak ayarlayın (Lokal):
    ```bash
    export GEMINI_API_KEY="SİZİN_API_ANAHTARINIZ"
    ```
    veya kodu çalıştırdığınızda istendiğinde yapıştırın.

### 3. Projeyi Başlatma

Aşağıdaki komut ile chatbot'u başlatın:

```bash
python biyoloji_rag_chatbot.py
