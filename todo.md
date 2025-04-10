# To-Do List: Building ContentDigest MVP

## Week 1: Setup and Basic Structure

### Set up the project environment
- [ ] Install Node.js and Python on your machine
- [ ] Create a new project directory: `mkdir contentdigest && cd contentdigest`
- [ ] Initialize a Git repository: `git init`

### Set up the frontend
- [ ] Create a React app: `npx create-react-app frontend`
- [ ] Navigate to the frontend directory: `cd frontend`
- [ ] Start the app to ensure it works: `npm start`
- [ ] Create basic components:
  - [ ] `InputForm.js`: For URL input
  - [ ] `Dashboard.js`: To display summaries
  - [ ] `SearchBar.js`: For searching summaries

### Set up the backend
- [ ] Create a backend directory: `mkdir backend && cd backend`
- [ ] Initialize a Python virtual environment: `python -m venv venv && source venv/bin/activate`
- [ ] Install Flask: `pip install flask`
- [ ] Create a basic Flask app in `app.py`:
```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello():
    return 'Backend is running!'
```
- [ ] Run the Flask app: `flask run` (ensure it's accessible at http://localhost:5000)

### Set up the database
- [ ] Install SQLite (already included with Python)
- [ ] Create a `database.py` file to initialize SQLite:
```python
import sqlite3

conn = sqlite3.connect('summaries.db')
cursor = conn.cursor()
cursor.execute('''CREATE TABLE IF NOT EXISTS summaries
                  (id INTEGER PRIMARY KEY, user_id INTEGER, url TEXT, 
                   summary TEXT, date_added TEXT)''')
conn.commit()
conn.close()
```
- [ ] Test the database by inserting a dummy record

## Week 2: AI Summarization Integration

### Integrate AI for summarization
- [ ] Install Hugging Face Transformers: `pip install transformers`
- [ ] Create a `summarize.py` file in the backend:
```python
from transformers import pipeline

summarizer = pipeline("summarization", model="facebook/bart-large-cnn")

def summarize_text(text):
    summary = summarizer(text, max_length=150, min_length=50, do_sample=False)
    return summary[0]['summary_text']
```
- [ ] Test the summarizer with a sample article text

### Add YouTube transcript fetching
- [ ] Install the YouTube Data API client: `pip install google-api-python-client`
- [ ] Get an API key from Google Cloud Console and enable the YouTube Data API
- [ ] Create a `youtube.py` file to fetch transcripts:
```python
from googleapiclient.discovery import build

api_key = "YOUR_API_KEY"
youtube = build('youtube', 'v3', developerKey=api_key)

def get_transcript(video_id):
    try:
        transcript = youtube.captions().list(part="snippet", videoId=video_id).execute()
        return transcript  # Simplified; parse as needed
    except:
        return None
```
- [ ] Test with a sample YouTube video URL

### Add web scraping for articles
- [ ] Install BeautifulSoup: `pip install beautifulsoup4 requests`
- [ ] Create a `scrape.py` file:
```python
import requests
from bs4 import BeautifulSoup

def scrape_article(url):
    response = requests.get(url)
    soup = BeautifulSoup(response.text, 'html.parser')
    paragraphs = soup.find_all('p')
    return ' '.join([p.text for p in paragraphs])
```
- [ ] Test with a sample article URL

### Create the summarization endpoint
- [ ] Add a `/submit-link` endpoint in `app.py`:
```python
from flask import request
import sqlite3
from summarize import summarize_text
from youtube import get_transcript
from scrape import scrape_article

@app.route('/submit-link', methods=['POST'])
def submit_link():
    url = request.json['url']
    user_id = request.json['user_id']  # Simplified; add auth later
    content = None

    if 'youtube.com' in url:
        video_id = url.split('v=')[1]
        transcript = get_transcript(video_id)
        if transcript:
            content = transcript
    else:
        content = scrape_article(url)  # Add PDF handling later

    if content:
        summary = summarize_text(content)
        conn = sqlite3.connect('summaries.db')
        cursor = conn.cursor()
        cursor.execute("""
            INSERT INTO summaries (user_id, url, summary, date_added) 
            VALUES (?, ?, ?, datetime('now'))
        """, (user_id, url, summary))
        conn.commit()
        conn.close()
        return {'status': 'success', 'summary': summary}
    return {'status': 'error', 'message': 'Could not process content'}
```
- [ ] Test the endpoint using Postman or curl

## Week 3: Search and Authentication

### Add user authentication
- [ ] Install Flask-Login: `pip install flask-login`
- [ ] Create a `users.py` file to manage users:
```python
import sqlite3

def create_user(email, password):
    conn = sqlite3.connect('summaries.db')
    cursor = conn.cursor()
    cursor.execute('''CREATE TABLE IF NOT EXISTS users
                      (id INTEGER PRIMARY KEY, email TEXT, password TEXT)''')
    cursor.execute("INSERT INTO users (email, password) VALUES (?, ?)", 
                  (email, password))
    conn.commit()
    conn.close()
```
- [ ] Add login/signup endpoints in `app.py`
- [ ] Implement password hashing and authentication logic
- [ ] Create login/signup forms in the frontend
- [ ] Add protected routes

### Add search functionality
- [ ] Create a search endpoint in the backend
- [ ] Implement basic keyword matching in SQLite
- [ ] Create a search results component in the frontend
- [ ] Test search functionality with various queries

## Week 4: Testing and Deployment

### Testing
- [ ] Write unit tests for backend functions
- [ ] Test all API endpoints
- [ ] Test frontend components
- [ ] Perform end-to-end testing
- [ ] Fix any bugs found during testing

### Deployment
- [ ] Set up Heroku account
- [ ] Configure deployment settings
- [ ] Deploy backend to Heroku
- [ ] Deploy frontend to Vercel
- [ ] Test the deployed application
- [ ] Monitor for any issues

### Documentation
- [ ] Write API documentation
- [ ] Create user guide
- [ ] Document deployment process
- [ ] Add README.md with setup instructions

### Final Steps
- [ ] Conduct security review
- [ ] Optimize performance
- [ ] Prepare for beta testing
- [ ] Create feedback collection mechanism
