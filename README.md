# 🏥 Medical RAG Question Answering System

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)

**Clinical Document QA con RAG ibrido e Fine-Tuning QLoRA** *Progetto per lo Short Master Universitario "Generative AI: dalla teoria alla pratica" - Università degli Studi di Bari Aldo Moro.*

**Autore:** Nicola Mastromarino

---

## 📖 Descrizione del Progetto

Questo repository contiene l'implementazione di un **Clinical Decision Support System (CDSS)** basato su LLM open-source specializzato per il dominio biomedico. Il sistema sfrutta un approccio **Retrieval-Augmented Generation (RAG)** per estrarre informazioni accurate da note cliniche in formato testo libero.

L'obiettivo principale è minimizzare le *hallucinations* (allucinazioni) tipiche dei modelli linguistici in contesti clinici critici e gestire in modo efficace il trade-off tra la correttezza della risposta e l'accuratezza nell'astensione (`NO_ANSWER`).

## 🏗️ Architettura del Sistema

Lo schema seguente illustra la biforcazione fondamentale tra il modulo di **Recupero delle Informazioni** (*Retriever*) e il modulo di **Generazione** (*Generator*), che costituisce il principio architetturale alla base del paradigma RAG.

<p align="center">
  <img src="https://raw.githubusercontent.com/NicolaM99/medical-rag-qa/refs/heads/main/architettura.svg" alt="Architettura del Sistema Medical RAG Question Answering" width="100%">
</p>

> **Lettura del diagramma:** La query clinica percorre simultaneamente due rami. Il *Retriever* recupera i frammenti più rilevanti dalla Knowledge Base (MIMIC-III) tramite Reciprocal Rank Fusion di BM25 e PubMedBERT; il *Generator* (BioMistral-7B ottimizzato con QLoRA) riceve la query e il contesto e produce la risposta finale o emette `NO_ANSWER` in assenza di evidenza.

### Stack Tecnologico
* **Modello Generativo**: `BioMistral/BioMistral-7B` ottimizzato con fine-tuning QLoRA (via Unsloth).
* **Retrieval**: Sistema ibrido dinamico che bilancia *Dense Retrieval* (`pritamdeka/S-PubMedBert-MS-MARCO` + FAISS) e *Sparse Retrieval* (Okapi BM25).
* **Dataset**: MIMIC-III de-identificato (~100.000 note cliniche reali in aderenza HIPAA).
* **Ambiente di Sviluppo**: Jupyter Notebook (testato su Kaggle 2x NVIDIA T4 GPU).

## 📂 Struttura del Repository

```text
medical-rag-qa/
├── architettura.svg                  # Diagramma architetturale del sistema
├── docs/
│   └── Master_Thesis_Medical_RAG.pdf # Relazione tecnica completa e metriche
├── notebooks/
│   └── medical_rag_pipeline.ipynb    # Pipeline principale (Retrieval, QLoRA, RAG)
├── .gitignore                        # File e cartelle ignorate da git
├── LICENSE                           # Licenza MIT
├── README.md                         # Documentazione del repository
└── requirements.txt                  # Dipendenze del progetto
```

## 🚀 Installazione ed Esecuzione

1. **Clona il repository:**
   ```bash
   git clone [https://github.com/NicolaM99/medical-rag-qa.git](https://github.com/NicolaM99/medical-rag-qa.git)
   cd medical-rag-qa
   ```

2. **Installa le dipendenze:**
   Si consiglia di utilizzare un ambiente virtuale (es. `venv` o `conda`).
   ```bash
   pip install -r requirements.txt
   ```
   *Nota: La pipeline sfrutta `unsloth` per ottimizzare le prestazioni e ridurre drasticamente il consumo di VRAM durante l'inferenza e il fine-tuning.*

3. **Avvia il notebook:**
   Il codice sorgente per l'estrazione, il retrieval ibrido e la generazione si trova all'interno della cartella dedicata.
   ```bash
   jupyter notebook notebooks/medical_rag_pipeline.ipynb
   ```

> ⚠️ **Nota sulla Privacy (Dataset)**: Il dataset MIMIC-III originale non è incluso in questo repository a causa delle rigorose policy d'uso (richiede l'accettazione del DUA e credenziali approvate da PhysioNet). Per testare il sistema in locale, è necessario utilizzare un campione mock o i propri dati clinici de-identificati.

## 📊 Risultati e Metriche

Tutti i dettagli riguardanti la metodologia sperimentale, le metriche adottate e il confronto tra i diversi esperimenti (Baseline vs Fine-Tuned) sono documentati in modo esaustivo nella [Relazione Tecnica (PDF)](docs/Master_Thesis_Medical_RAG.pdf). Le metriche principali valutate includono:
* Exact Match (EM)
* Token F1 Score
* ROUGE-L
* Hallucination Rate
* No-Answer Accuracy

## 📜 Licenza
Questo progetto è distribuito con licenza **MIT**. Consulta il file `LICENSE` per maggiori informazioni.
