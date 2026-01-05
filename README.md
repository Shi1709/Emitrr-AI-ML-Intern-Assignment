# 🩺 Physician Notetaker — NLP Pipeline

This project was built in Google Colab as an NLP pipeline for analysing a physician–patient conversation. The goal was to extract key medical details from the transcript, generate a structured summary, detect the patient’s sentiment and intent, and also produce a simple SOAP-style note.

###  Setup Instructions (Google Colab)
1️⃣ Open Google Colab

  Create a new notebook at:
  
  https://colab.research.google.com

2️⃣ Paste the code cells in order

  The notebook is organised into small sections:
  
  installing dependencies
  
  loading the NLP and transformer models
  
  helper functions
  
  rule-based medical NER
  
  structured JSON summary
  
  summarisation
  
  sentiment & intent (focused on patient speech)
  
  SOAP note generator
  
  pipeline runner
  
  transcript input
  
  I just ran each cell one by one.

3️⃣ Run the pipeline

  In the last cell, I paste the transcript and run:
  
    text = """
    <PASTE TRANSCRIPT HERE>
    """
    run_pipeline(text)
    
  
  It prints:
  
  the structured medical summary
  
  keywords
  
  sentiment + intent output
  
  and the generated SOAP note 

###  How I would train an NLP model to map transcripts into SOAP format

  If I were actually training a model to convert transcripts into SOAP notes, I would treat it as a supervised learning problem where the model learns from examples of real consultation notes.
  
  The idea would be to use a dataset where each transcript is paired with a clinician-written SOAP note, and then label parts of the conversation as:
  
  Subjective — how the patient describes symptoms and history
  
  Objective — exam-related findings and observations
  
  Assessment — diagnosis or clinical judgement
  
  Plan — treatment advice, physiotherapy, medication, follow-up
  
  I would probably try two modelling approaches:
  one using a sequence-to-sequence model (like T5 or BART) to generate the whole SOAP note, and another using a sentence-level classifier (such as BioClinicalBERT) to assign each line to the correct section.
  
  Speaker roles also help — patient speech mostly belongs in Subjective, while the doctor’s lines usually contribute more to Objective, Assessment, and Plan. I’d judge the model based on whether the information is grouped correctly and whether important medical details are preserved.

###  Rule-based vs deep-learning techniques to improve accuracy
  
  In practice, I think the most reliable way to generate SOAP notes would be to combine both rule-based logic and deep-learning models instead of relying on only one approach.
  
  Rule-based patterns help with structure and clinical safety — for example:
  
  pain, discomfort, stiffness → Subjective
  
  physical examination comments → Objective
  
  diagnosis or prognosis statements → Assessment
  
  treatment or follow-up instructions → Plan
  
  Negation handling (like “no tenderness”) and words that indicate time or recovery (such as “improving” or “occasional”) also make the output more accurate.
  
  Deep-learning models such as SciSpaCy or BioClinicalBERT would help detect medical terms and classify sentences more reliably. I would still keep a final validation step so that no information is placed in the wrong section and nothing is added beyond what is actually said in the transcript.
  
  Overall, a hybrid approach — rules for precision and structure, and models for understanding language — would produce cleaner, safer and more meaningful SOAP notes.
