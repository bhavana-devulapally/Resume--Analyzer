
# Resume Analyzer

A lightweight **NLP-powered** microservice for automated resume–to–job-description matching. Built on Flask and scikit-learn, it pipelines raw CV documents through text preprocessing, vectorization, and cosine-based retrieval to surface top candidates.

---

## Core Features

- **Multi-format Ingestion**: Accepts PDF, DOCX, and TXT resumes via HTTP file upload.  
- **Text Normalization**:  
  - **Tokenization** (whitespace & punctuation)  
  - **Lemmatization** (NLTK/ spaCy)  
  - **Stop-word Filtering**  
  - **Lowercasing & Punctuation Stripping**  
- **Vector Embeddings**:  
  - Builds a **TF-IDF** document-term matrix for both resumes and the job description.  
  - Supports easy swap to **contextual embeddings** (e.g., BERT, RoBERTa) in future.  
- **Similarity Scoring**: Computes **cosine similarity** between the job description vector and each resume vector, then ranks top 3 matches.  
- **RESTful API**: Exposes `/analyze` endpoint for JSON-based requests and responses.  
- **Containerized Deployment**: Ready for Docker, deployable on Heroku, PythonAnywhere, or any cloud container service.

---

## System Architecture

1. **Upload Service**  
   - Receives multipart/form-data with up to 10 resume files and a plain-text job description.  
2. **NLP Preprocessing Module**  
   - Sequential pipeline: tokenization → stop-word removal → lemmatization → sentence segmentation.  
3. **Vectorization Layer**  
   - `TfidfVectorizer` fits on combined corpus (all resumes + job description).  
4. **Similarity Engine**  
   - Projects documents into a **sparse vector space**.  
   - Calculates pairwise **cosine scores** and sorts descending.  
5. **Response Formatter**  
   - Returns JSON array of `{ filename, similarity_score }` objects, truncated to the top 3.

---

## Quick Start

1. **Clone & Install**  
   ```bash
   git clone https://github.com/SeekAI-786/Resume-Analyzer.git
   cd Resume-Analyzer
   pip install -r requirements.txt
