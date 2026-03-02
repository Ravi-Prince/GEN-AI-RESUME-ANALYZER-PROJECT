# GEN-AI-RESUME-ANALYZER-PROJECT


## Installing Application Dependencies

!pip install streamlit pyngrok pdfplumber

This command installs the required libraries for the app:

- `streamlit`  
  Builds and runs the web interface.

- `pyngrok`  
  Creates a public tunnel so the Streamlit app can be accessed from Colab.

- `pdfplumber`  
  Extracts text from uploaded PDF lecture files.

These packages support the interface, deployment, and PDF processing
components of the project.

## Resume Analyzer Application (Streamlit)

This script creates a simple Resume Analyzer web app using Streamlit.

The application:

1. Accepts a resume in PDF format  
2. Extracts text from the PDF  
3. Performs basic rule-based analysis  
4. Generates a score and feedback  

---

### %%writefile app.py

Writes the following code into a file named `app.py`.

This allows the app to be launched using:
`streamlit run app.py`

---

### Libraries Used

- `streamlit`  
  Builds the web interface.

- `pdfplumber`  
  Extracts text from uploaded PDF resumes.

- `re`  
  Performs pattern matching using regular expressions.

---

### Resume Upload

`st.file_uploader()` allows users to upload a PDF resume.

Only PDF files are accepted.

---

### Text Extraction

`extract_text(file)`

- Opens the PDF
- Extracts text from each page
- Combines all pages into one string

If a page has no readable text, it safely returns an empty string.

---

### Resume Analysis Logic

`analyze_resume(text)`

This is a rule-based scoring system.

#### 1. Skills Detection
Checks if predefined skills appear in the resume:
- python
- java
- c++
- machine learning
- data analysis

Each detected skill adds 10 points.

#### 2. Email Detection
Uses a regular expression to check for a valid email format.
Adds 10 points if found.

#### 3. Phone Number Detection
Checks for a 10-digit number pattern.
Adds 10 points if found.

The function returns:
- Total score
- List of feedback messages

---

### Output Display

If a resume is uploaded:

- Displays extracted text preview (first 1000 characters)
- Shows calculated resume score
- Displays structured feedback

---

## Important Notes

- This is a basic keyword-based scoring system.
- It does not evaluate experience quality or project depth.
- It only checks presence of keywords and contact details.
- Scanned image-based PDFs will not work without OCR.


## Launching the Resume Analyzer App

!streamlit run app.py &>/content/logs.txt &

This command starts the Streamlit application in the background.

- `streamlit run app.py`  
  Runs the Resume Analyzer app (default port 8501).

- `&> /content/logs.txt`  
  Redirects all output and errors to a log file to keep the notebook clean.

- `&`  
  Runs the server in the background so the notebook can continue executing other cells.

This step is required in Colab before exposing the app using ngrok.



## Configuring ngrok Authentication

!ngrok config add-authtoken YOUR_AUTHTOKEN

This command links ngrok in Colab to your personal ngrok account.

Why this is required:

- Anonymous ngrok sessions have strict limits.
- Adding an authtoken unlocks your account permissions.
- It allows you to create and manage tunnels from your dashboard.

Important:
The authtoken is a private credential and should never be shared
or committed to public repositories.


## Exposing the App with ngrok

from pyngrok import ngrok
public_url = ngrok.connect(8501)
print(public_url)

This code creates a public tunnel to the Streamlit app.

- `ngrok.connect(8501)`  
  Opens a secure tunnel to port 8501 (default Streamlit port).

- `public_url`  
  Stores the generated public link.

- `print(public_url)`  
  Displays the link so the app can be accessed from outside Colab.

This step is necessary because Colab runs on a private environment,
and localhost is not publicly accessible.
