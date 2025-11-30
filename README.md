DiReCT: Diagnostic Reasoning for Clinical Notes 🏥

DiReCT (Diagnostic Reasoning for Clinical Notes) is a privacy-first Retrieval Augmented Generation (RAG) system designed to perform diagnostic reasoning on the MIMIC-IV-Ext dataset.

Unlike standard RAG implementations, this project features a Recursive Hierarchical Crawler that preserves the directory structure of clinical notes (e.g., Asthma > Severe Asthma Exacerbation), injecting critical disease context into the vector embeddings.

🌟 Key Features

Hierarchical Context Awareness: Automatically crawls nested patient folders and uses the folder path as diagnostic metadata.

Privacy-First Architecture: Runs entirely locally using Llama 3 (via Ollama) and FAISS, ensuring no patient data leaves the environment.

Clinical Note Parsing: Specialized parsing logic to stitch together segmented JSON fields (input1 for Chief Complaint, input2 for History, etc.) into coherent medical narratives.

Interactive UI: Built with Streamlit for a seamless query experience.

🛠️ Tech Stack

LLM: Llama 3 (8B) via Ollama

Orchestration: LangChain

Vector Store: FAISS (Facebook AI Similarity Search)

Embeddings: sentence-transformers/all-MiniLM-L6-v2

Frontend: Streamlit

🚀 Installation

Clone the repository

git clone [https://github.com/your-username/DiReCT-Clinical-RAG.git](https://github.com/your-username/DiReCT-Clinical-RAG.git)
cd DiReCT-Clinical-RAG


Install Python Dependencies

pip install -r requirements.txt


Setup Local LLM (Ollama)

Download Ollama.

Run ollama pull llama3.

Start the server: ollama serve.

Run the App

streamlit run app.py


📂 Usage

Launch the app.

Paste the absolute path to your MIMIC-IV Finished folder.

Click Load Data. The system will recursively index all sub-folders.

Ask questions like: "What is the typical presentation of a patient with Severe Asthma compared to Non-Allergic Asthma?"

PART 2: Medium Blog Post Draft

(Copy this into your Medium Story editor)

Title: Building DiReCT: A Recursive RAG System for Hierarchical Clinical Data

Subtitle: How I solved nested data challenges in the MIMIC-IV dataset using Llama 3, LangChain, and Streamlit.

![Insert a screenshot of your Streamlit App here]

Introduction
In the world of healthcare AI, context is everything. A note mentioning "shortness of breath" means something very different if it's found in a "Heart Failure" file versus an "Asthma" file.

For my latest project, DiReCT (Diagnostic Reasoning for Clinical Notes), I was tasked with building a RAG system for the MIMIC-IV-Ext dataset. However, I quickly realized that standard "flat" retrieval methods wouldn't work. The dataset was highly nested, with folders representing disease hierarchies (e.g., Asthma > Severe > Exacerbation).

Here is how I built a privacy-aware, context-sensitive RAG system to solve this.

The Challenge: The "Lost in Hierarchy" Problem
Most RAG tutorials teach you to dump all text files into a vector store. If I did that, the AI would lose the critical information implied by the folder names. A patient file named 103.json inside the Severe Asthma folder doesn't always explicitly say "Severe Asthma" in the text—it assumes the doctor knows the context.

The Solution: Recursive Context Injection
I engineered a custom ingestion pipeline using Python and LangChain.

The Crawler: Instead of a simple file loader, I wrote a recursive os.walk function. It traverses the directory tree and calculates the relative path for every file.

Context Injection: Before embedding the text, the system injects this path as a header.

Raw Text: "Patient complains of wheezing."

Processed Text: "Condition Category: Asthma > Severe. Patient complains of wheezing."

This small change had a massive impact. The vector database (FAISS) now understands that this specific "wheezing" is mathematically related to "Severe Asthma."

Parsing MIMIC-IV JSONs
Another hurdle was the data format. The clinical notes were split across multiple JSON keys (input1 for symptoms, input2 for history, etc.). I implemented a robust parser that stitches these segments together into a single, coherent clinical narrative before indexing.

Privacy by Design with Llama 3
Handling clinical data requires strict privacy. Sending patient notes to a public API (like GPT-4) is often a violation of protocol. I architected DiReCT to run entirely on Llama 3 via Ollama. This ensures that the inference happens locally on the machine, making it a viable architecture for real-world hospital pilots.

Results
The final system allows users to ask complex diagnostic questions. When asked, "How do symptoms differ between Bacterial and Viral Pneumonia?", the system successfully retrieves documents from the respective sub-folders and synthesizes an answer citing the specific patient files.

Conclusion
DiReCT demonstrates that effective RAG isn't just about the model—it's about how you engineer the data before it ever reaches the model. By preserving the hierarchical structure of clinical records, we can build AI assistants that reason like clinicians.
