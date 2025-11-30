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

