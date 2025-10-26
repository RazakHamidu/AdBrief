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

{
  "inputData": "Analisi brand: GreenWave produce abbigliamento sportivo sostenibile. Target 25-40 anni, urbano e attento all’ambiente. USP: materiali riciclati, design minimalista. Obiettivo: awareness + vendite online."
}

### Output (Markdown)

# 📋 BRIEF CREATIVO - GreenWave Momentum

**Client:** GreenWave  
**Campaign:** “Muoviti in modo sostenibile”  
**Date:** 26/10/2025  

## 🎯 TARGET AUDIENCE

Primary Target: Giovani adulti (25-40 anni), urbani e attenti alla sostenibilità
Pain Points: Mancanza di alternative eco-fashion accessibili
Aspirazioni: Stile, etica, autenticità
Comportamenti Digitali: Instagram, TikTok, YouTube Fitness


## 📢 MESSAGGIO CHIAVE

USP: Abbigliamento sportivo 100% riciclato
Tone of Voice: Naturale, motivazionale, positivo
Value Proposition: Stile che rispetta l’ambiente senza rinunciare alle performance


## 🚀 STRATEGIA PER PIATTAFORMA

### GOOGLE ADS
- **Keyword Themes:** moda sostenibile, abbigliamento ecologico, green fitness  
- **Ad Copy Angle:** prestazioni + sostenibilità  
- **Landing Page Focus:** materiali riciclati, certificazioni, comfort  

### META ADS  
- **Audience Targeting:** interessi in eco-lifestyle e sport  
- **Creative Strategy:** immagini naturali, colori neutri, storytelling visivo  
- **Placement Priority:** Instagram Reels, Facebook Feed  

### TIKTOK ADS  
- **Content Approach:** video “before/after” moda fast fashion → green fashion  
- **Hook Strategy:** “Non serve scegliere tra stile e sostenibilità”  
- **Sound Strategy:** musica pop soft + suoni naturali  

## 📊 KPI E METRICS
- _Google_: CTR > 4%, Conversion Rate > 3%  
- _Meta_: CPC < €0.50, Engagement > 6%  
- _TikTok_: Completion Rate > 70%, CPM < €3  

## ⏰ TIMELINE CONSIGLIATA
- **Week 1-2:** awareness e storytelling  
- **Week 3-4:** scaling su audience lookalike  

## 💡 CALL-TO-ACTION
- **Google:** Scopri la collezione  
- **Meta:** Acquista ora  
- **TikTok:** Unisciti al movimento #GreenWave  

---

## 🔐 Credenziali richieste

| Servizio                 | Tipo credenziale | Descrizione                                  |
| ------------------------ | ---------------- | -------------------------------------------- |
| Google Gemini (PaLM API) | googlePalmApi  | Accesso al modello LLM per generazione testo |

---

## 🧪 Test e Debugging

Puoi testare il workflow localmente o su server con una chiamata HTTP:

curl -X POST https://<TUO_DOMINIO_N8N>/webhook/test1 \
  -H "Content-Type: application/json" \
  -d '{
        "inputData": "Analisi del mercato fitness tech in Europa 2025, con focus su wearable e app di performance..."
      }'

La risposta sarà un file di testo in formato Markdown contenente il brief completo.

---

## 🧰 Requisiti tecnici

* n8n ≥ 1.50
* Plugin LangChain abilitato
* Credenziale attiva per Google Gemini API
* Connettività HTTPS per Webhook

---

## 🚀 Possibili estensioni

* Aggiungere un nodo Save to Google Docs / Notion / Airtable
* Creare un nodo Email Send per inviare il brief al team creativo
* Integrazione con Slack per notifiche automatiche
* Aggiungere una fase di revisione AI per migliorare tono e consistenza

---

## 🏁 Conclusione

AdBrief Generator è una soluzione di automazione strategica per team marketing e creativi:
trasforma dati e analisi in brief concreti, pronti all’uso e coerenti con i canali pubblicitari più importanti.

> 💡 In pochi secondi, da analisi grezza a piano creativo operativo.
> 100% personalizzabile, scalabile e integrabile con qualsiasi ecosistema AI o CRM.

---
