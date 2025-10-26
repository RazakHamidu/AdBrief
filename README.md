---

# 📋 AdBrief Generator

### Workflow n8n per la generazione automatica di brief creativi e strategici con AI

---

## 🧠 Panoramica generale

AdBrief Generator è un workflow sviluppato in n8n che utilizza Google Gemini (PaLM API) per trasformare un testo di ricerca o analisi di mercato in un brief pubblicitario strutturato, pratico e operativo.

Il sistema analizza automaticamente i dati in ingresso (descrizioni di brand, insight, analisi di mercato, report) e produce un documento di brief completo in linguaggio naturale, formattato in Markdown, con raccomandazioni concrete per campagne digitali su Google Ads, Meta Ads e TikTok Ads.

---

## ⚙️ Architettura del Workflow

| Nodo                            | Tipo                                          | Ruolo principale                                                   |
| ------------------------------- | --------------------------------------------- | ------------------------------------------------------------------ |
| 🟢 Webhook                  | n8n-nodes-base.webhook                      | Riceve la richiesta HTTP con i dati di input da analizzare         |
| 🧩 AdBrief Generator        | @n8n/n8n-nodes-langchain.chainLlm           | Catena AI principale: elabora il prompt e genera il brief          |
| 🔵 Google Gemini Chat Model | @n8n/n8n-nodes-langchain.lmChatGoogleGemini | Modello LLM di Google usato come motore di generazione linguistica |
| 📤 Respond to Webhook       | n8n-nodes-base.respondToWebhook             | Restituisce il brief generato in formato Markdown                  |

---

## 🧩 Flusso logico step-by-step

### 1️⃣ Ricezione dell’input (Webhook)

* Endpoint HTTP configurato su:

  
  POST /webhook/test1
  
* Il Webhook riceve il campo inputData nel body JSON.
  Esempio:

    {
    "inputData": "Analisi di mercato del settore moda sostenibile in Italia 2025. Target giovane adulto 25-35 anni, interessato a stile, etica e autenticità..."
  }
  
* Questi dati vengono passati al nodo AdBrief Generator.

---

### 2️⃣ Elaborazione AI (AdBrief Generator)

Il nodo AdBrief Generator è la logica centrale.
È configurato per:

* Analizzare i dati di input
* Estrarre insight chiave (target, pain points, USP, competitor, obiettivi marketing)
* Strutturare un brief pubblicitario dettagliato e formattato in Markdown

#### Prompt principale

Il prompt guida il modello LLM seguendo regole precise di analisi e output:

Contenuti principali del prompt:

* Identità AI: strategist pubblicitario esperto in campagne digitali multipiattaforma
* Fasi operative:

  1. Analisi del testo in input
  2. Estrazione di target, USP, obiettivi e contesto competitivo
  3. Strutturazione del brief creativo
* Output richiesto: documento Markdown con sezioni fisse:

  * 🎯 Target Audience
  * 📢 Messaggio Chiave
  * 🚀 Strategia per piattaforma (Google, Meta, TikTok)
  * 📊 KPI e Metrics
  * ⏰ Timeline consigliata
  * 💡 Call-to-Action
* Regole di stile:

  * Linguaggio professionale ma accessibile
  * Massimo 1000 parole
  * Tono pratico, data-driven, creativo
  * Nessuna invenzione di dati inesistenti

---

### 3️⃣ Generazione linguistica (Google Gemini Chat Model)

* Motore LLM: Google Gemini (PaLM API)
* Funzione: interpretare il prompt e generare il testo del brief con alta coerenza strutturale e linguistica.
* Credenziali richieste:

    googlePalmApi:
    name: Google Gemini (PaLM) API account
  
* Parametri:

  * Input: inputData (testo di analisi)
  * Output: brief completo in Markdown

---

### 4️⃣ Restituzione del risultato (Respond to Webhook)

* Il nodo Respond to Webhook restituisce la risposta direttamente al client che ha inviato la richiesta.
* La risposta è un testo in formato Markdown, pronto per essere:

  * Copiato in documenti creativi o Notion
  * Inserito in CRM o dashboard marketing
  * Convertito in PDF o HTML per report

---

## 🧾 Esempio di input/output

### Input (via POST)
