# Resume Analyzer

An NLP Flask microservice for automated resume-to-job-description matching.

---

##  Project Overview

Resume Analyzer streamlines candidate screening by ingesting up to 10 resumes (PDF, DOCX, TXT) and a plain-text job description, then running a full NLP pipeline—text extraction, normalization, TF-IDF vectorization and cosine similarity—to rank the top 3 most relevant resumes in under a second.

---

##  NLP Pipeline

1. **Document Ingestion**  
   - PDF via **PyPDF2**  
   - DOCX via **docx2txt**  
   - TXT via standard file read  

2. **Text Preprocessing**  
   - Lowercasing & punctuation stripping  
   - **Tokenization** (splitting on whitespace & punctuation)  
   - **Stop-word removal** using NLTK’s English stoplist  
   - (_Optional: lemmatization with spaCy or NLTK_)  

3. **Vectorization**  
   - Build a **TF-IDF** document-term matrix over the job description + all resumes  

4. **Similarity Retrieval**  
   - Project documents into a sparse vector space  
   - Compute **cosine similarity** scores between the job description vector and each resume vector  
   - Sort descending and return the **top 3** matches  

---

## ⚙ Installation

```bash
# 1. Clone the repository
git clone https://github.com/SeekAI-786/Resume-Analyzer.git
cd Resume-Analyzer

# 2. Create & activate a Python virtual environment
python3 -m venv venv
source venv/bin/activate

# 3. Install dependencies
pip install -r requirements.txt
````

---

## ▶ Running Locally

```bash
python main.py
```

* The service will listen at `http://127.0.0.1:5000/`
* Use the web form or send a POST request to `/upload` with:

  * **`resumeText`**: plain-text job description
  * **`resumeFile`**: up to 10 resume files (PDF, DOCX, TXT)

---

##  API Example

```bash
curl -X POST http://localhost:5000/upload \
  -F "resumeText=Seeking a Senior NLP Engineer with Python & deep learning expertise" \
  -F "resumeFile=@alice_resume.pdf" \
  -F "resumeFile=@bob_cv.docx"
```

**Sample HTML Response**

```html
<h3>Top Matching Resumes:</h3>
<div>Alice_resume.pdf — 0.84</div>
<div>Bob_cv.docx    — 0.77</div>
<div>Carol.txt      — 0.65</div>
```

---

##  Project Structure

```
Resume-Analyzer/
├─ main.py             # Flask app & NLP pipeline
├─ requirements.txt    # Python dependencies
├─ templates/
│  └─ app.html         # Upload form + results template
├─ uploads/            # Temporary resume storage


```

---

##  Future Extensions

* Swap TF-IDF for **contextual embeddings** ( BERT, Sentence-Transformers)
* Integrate **lemmatization** and POS-based filtering for cleaner token sets
* Add **skill-based filtering** using spaCy’s Matcher or regex rules
* Expose a **pure JSON REST API** (no HTML) for CI/CD integration
* Build an interactive **dashboard** with similarity heatmaps and audit logs




```
