# Aurora_Voice

**Aurora_Voice** is a comprehensive AI-powered technical interview preparation and automation suite designed to streamline the interview process for both candidates and interviewers. The system combines resume analysis, intelligent question generation, voice interaction capabilities, and detailed performance assessment to create a complete interview solution.

## 🌟 Key Features

- **AI-Powered Resume Analysis**: Automatically extract technical skills and keywords from PDF resumes
- **Dynamic Question Generation**: Create tailored technical interview questions using AWS Bedrock (Claude 3 Sonnet)
- **Voice-Enabled Interviews**: Conduct interviews with text-to-speech and speech recognition capabilities
- **Comprehensive Analysis**: Generate detailed feedback reports and performance assessments
- **Session Management**: Track and manage multiple interview sessions with persistent data storage
- **Web-Based Interface**: User-friendly web applications for all components

## 🔧 System Architecture

Aurora_Voice consists of three interconnected Python applications that work together to provide a complete interview workflow:

```
┌─────────────────┐    ┌─────────────────┐     ┌────────────────────┐
│  thinking_app   │───▶│  talking_app    │───▶│interview_analyzer  │
│   (Port 5000)   │    │   (Port 5001)   │     │   (Command Line)   │
│                 │    │                 │     │                    │
│ • Resume Upload │    │ • Voice Interface│    │ • Session Analysis │
│ • Skill Extract │    │ • Q&A Recording │     │ • Report Generation│
│ • Question Gen  │    │ • TTS/STT       │     │ • Feedback System  │
└─────────────────┘    └─────────────────┘     └────────────────────┘
         │                       │                       │
         └───────────────────────┼───────────────────────┘
                                 ▼
                    ┌─────────────────────┐
                    │  shared_questions/  │
                    │                     │
                    │ • Session Data      │
                    │ • Questions         │
                    │ • Answers           │
                    │ • Metadata          │
                    └─────────────────────┘
```

## 📋 Detailed Component Overview

### 1. `thinking_app.py` - Resume Analysis & Question Generation
**Port:** 5000  
**Purpose:** Frontend for resume processing and interview preparation

#### Core Features:
- **PDF Resume Upload**: Secure file upload with validation
- **Technical Skill Extraction**: NLP-based keyword identification for programming languages, frameworks, tools, and technologies
- **AI Question Generation**: Uses AWS Bedrock Claude 3 Sonnet to create 5 tailored technical questions based on extracted skills
- **Session Management**: Creates unique session IDs for tracking interview progress
- **Data Persistence**: Saves questions and metadata to `shared_questions/` directory

#### Workflow:
1. User uploads PDF resume via web interface
2. System extracts text content from PDF
3. AI analyzes content to identify technical skills
4. AWS Bedrock generates relevant interview questions
5. Questions are saved with unique session ID
6. User receives session ID for next phase

#### Key Functions:
- `extract_text_from_pdf()`: PDF text extraction
- `extract_technical_skills()`: Skill identification algorithm
- `generate_questions()`: AWS Bedrock integration
- `save_session_data()`: Data persistence

### 2. `talking_app.py` - Voice-Enabled Interview Conductor
**Port:** 5001  
**Purpose:** Interactive voice interview session management

#### Core Features:
- **Text-to-Speech (TTS)**: Reads questions aloud using Google TTS
- **Speech Recognition**: Captures and transcribes candidate responses
- **Real-time Interaction**: Live audio feedback and progress tracking
- **Session Continuity**: Loads questions from `thinking_app.py` sessions
- **Answer Recording**: Saves both audio and transcribed text responses

#### Workflow:
1. User enters session ID from `thinking_app.py`
2. System loads corresponding questions
3. Application reads each question aloud
4. Candidate responds verbally
5. System transcribes and saves answers
6. Progress tracking through all questions
7. Session completion confirmation

#### Technical Components:
- **Audio Processing**: `pygame` for audio playback
- **Speech Recognition**: `SpeechRecognition` library with microphone input
- **TTS Engine**: Google Text-to-Speech (`gtts`)
- **Session Management**: File-based state tracking

### 3. `interview_analyzer.py` - Performance Analysis & Reporting
**Purpose:** Post-interview analysis and comprehensive reporting

#### Core Features:
- **Session Analysis**: Loads completed Q&A sessions
- **AI-Powered Feedback**: Uses AWS Bedrock to analyze answer quality
- **Performance Scoring**: Quantitative assessment of responses
- **Detailed Reports**: Comprehensive PDF/text reports with recommendations
- **Batch Processing**: Analyze multiple sessions simultaneously

#### Analysis Metrics:
- **Technical Accuracy**: Correctness of technical responses
- **Communication Clarity**: Articulation and explanation quality
- **Problem-Solving Approach**: Methodology and reasoning assessment
- **Knowledge Depth**: Understanding level evaluation
- **Overall Readiness**: Interview performance summary

#### Report Generation:
- **Executive Summary**: High-level performance overview
- **Question-by-Question Analysis**: Detailed feedback per question
- **Strengths & Weaknesses**: Identified areas of excellence and improvement
- **Recommendations**: Actionable advice for skill development
- **Score Breakdown**: Quantitative performance metrics

## 🔄 Complete Integration Workflow

### Phase 1: Resume Analysis (thinking_app.py)
```bash
# Start the resume analysis server
python thinking_app.py
```
1. Navigate to `http://localhost:5000`
2. Upload PDF resume
3. Review extracted technical skills
4. Generate tailored interview questions
5. **Save the session ID** provided

### Phase 2: Interview Conduct (talking_app.py)
```bash
# Start the voice interview server
python talking_app.py
```
1. Navigate to `http://localhost:5001`
2. Enter session ID from Phase 1
3. Follow voice prompts for each question
4. Speak clearly for accurate transcription
5. Complete all questions in the session

### Phase 3: Performance Analysis (interview_analyzer.py)
```bash
# Run the analysis tool
python interview_analyzer.py
```
1. Select completed session from list
2. Choose analysis type (feedback/report)
3. Review generated insights
4. Access saved reports in `reports/` directory

### Data Flow Integration
```
Resume (PDF) ──┐
               ├─▶ thinking_app.py ──┐
Skills Extracted ──┘                  │
                                      ▼
                              shared_questions/
                                ├── session_001.json
                                ├── session_002.json
                                └── ...
                                      ▲
                                      │
Audio Responses ──┐                   │
                  ├─▶ talking_app.py ──┘
Transcribed Text ──┘
                                      │
                                      ▼
                            interview_analyzer.py
                                      │
                                      ▼
                                 reports/
                                ├── analysis_001.pdf
                                ├── feedback_001.txt
                                └── ...
```

## 🛠️ Installation & Setup

### Prerequisites
- Python 3.8+
- AWS Account with Bedrock access
- Internet connection for TTS/STT services
- Microphone for voice input
- Audio output device

### 1. Environment Setup
```bash
# Clone the repository
git clone <repository-url>
cd Aurora_Voice

# Create virtual environment (recommended)
python -m venv aurora_env
source aurora_env/bin/activate  # On Windows: aurora_env\Scripts\activate

# Install core dependencies
pip install flask PyPDF2 boto3 gtts pygame SpeechRecognition

# Install additional audio dependencies (OS-specific)
# Ubuntu/Debian:
sudo apt-get install portaudio19-dev python3-pyaudio
pip install pyaudio

# macOS:
brew install portaudio
pip install pyaudio

# Windows:
# Download and install PyAudio wheel from:
# https://www.lfd.uci.edu/~gohlke/pythonlibs/#pyaudio
```

### 2. AWS Configuration
```bash
# Method 1: AWS CLI configuration
aws configure
# Enter your AWS Access Key ID
# Enter your AWS Secret Access Key
# Enter your default region (e.g., us-east-1)
# Enter your default output format (json)

# Method 2: Environment variables
export AWS_ACCESS_KEY_ID="your-access-key"
export AWS_SECRET_ACCESS_KEY="your-secret-key"
export AWS_DEFAULT_REGION="us-east-1"
```

### 3. AWS Bedrock Model Access
Ensure you have access to the Claude 3 Sonnet model in AWS Bedrock:
1. Navigate to AWS Bedrock console
2. Go to "Model access" in the left sidebar
3. Request access to "Anthropic Claude 3 Sonnet" if not already enabled
4. Wait for approval (usually immediate for standard models)

### 4. Directory Structure Setup
```bash
# Create necessary directories
mkdir -p shared_questions
mkdir -p reports
mkdir -p templates
mkdir -p uploads  # For temporary file storage
```

## 📁 Project Structure
```
Aurora_Voice/
├── interview_analyzer.py      # Analyze and report on interview sessions
├── thinking_app.py            # Resume analysis and question generation web app
├── talking_app.py             # Voice-enabled interview web app
├── README.md                  # Project documentation
├── shared_questions/          # Stores generated questions and answers (JSON files)
├── templates/                 # HTML templates for the web interfaces
│   ├── analysis.html          # Template for analysis/reporting
│   ├── thinking_index.html    # Main UI for resume analysis/question generation
│   └── talking_index.html     # Main UI for voice interview
├── temp/                      # Temporary files (e.g., TTS audio)
│   └── tts_*.mp3              # Example: temporary audio file
├── requirements.txt              # Python dependencies
├── config.py                     # Configuration settings
├── utils/                        # Utility functions
│   ├── __init__.py
│   ├── pdf_processor.py          # PDF handling utilities
│   ├── audio_processor.py        # Audio processing utilities
│   └── aws_client.py             # AWS Bedrock client wrapper
└── logs/                         # Application logs
```

## ⚙️ Configuration Options

### Environment Variables
```bash
# AWS Configuration
AWS_ACCESS_KEY_ID=your_access_key
AWS_SECRET_ACCESS_KEY=your_secret_key
AWS_DEFAULT_REGION=us-east-1

# Application Settings
FLASK_ENV=development
FLASK_DEBUG=True
MAX_CONTENT_LENGTH=16777216  # 16MB max file size

# Audio Settings
TTS_LANGUAGE=en
SPEECH_TIMEOUT=30  # seconds
SPEECH_PHRASE_TIMEOUT=5  # seconds

# Bedrock Settings
BEDROCK_MODEL_ID=anthropic.claude-3-sonnet-20240229-v1:0
BEDROCK_MAX_TOKENS=4096
BEDROCK_TEMPERATURE=0.7
```

### Application Configuration (config.py)
```python
import os

class Config:
    # Flask settings
    SECRET_KEY = os.environ.get('SECRET_KEY') or 'aurora-voice-secret-key'
    MAX_CONTENT_LENGTH = 16 * 1024 * 1024  # 16MB max file size
    
    # AWS settings
    AWS_REGION = os.environ.get('AWS_DEFAULT_REGION', 'us-east-1')
    BEDROCK_MODEL_ID = 'anthropic.claude-3-sonnet-20240229-v1:0'
    
    # Audio settings
    TTS_LANGUAGE = 'en'
    SPEECH_TIMEOUT = 30
    SPEECH_PHRASE_TIMEOUT = 5
    
    # File paths
    SHARED_QUESTIONS_DIR = 'shared_questions'
    REPORTS_DIR = 'reports'
    UPLOADS_DIR = 'uploads'
```

## 🚀 Advanced Usage

### Batch Processing Multiple Resumes
```python
# Create a batch processing script
import os
from thinking_app import process_resume

def batch_process_resumes(resume_directory):
    for filename in os.listdir(resume_directory):
        if filename.endswith('.pdf'):
            file_path = os.path.join(resume_directory, filename)
            session_id = process_resume(file_path)
            print(f"Processed {filename} -> Session ID: {session_id}")

# Usage
batch_process_resumes('resumes_to_process/')
```

### Custom Question Templates
```python
# Modify question generation prompts in thinking_app.py
CUSTOM_QUESTION_PROMPT = """
Based on the following technical skills: {skills}
Generate 5 advanced technical interview questions that:
1. Test practical application knowledge
2. Include scenario-based problems
3. Assess system design thinking
4. Evaluate debugging skills
5. Check optimization awareness

Format each question with:
- Question text
- Expected skill level (Junior/Mid/Senior)
- Key points to look for in answers
"""
```

### Integration with External Systems
```python
# Example: Slack integration for notifications
import requests

def send_slack_notification(session_id, status):
    webhook_url = "YOUR_SLACK_WEBHOOK_URL"
    message = {
        "text": f"Interview session {session_id} status: {status}"
    }
    requests.post(webhook_url, json=message)

# Use in your workflow
send_slack_notification("session_001", "completed")
```

## 🔧 Troubleshooting

### Common Issues and Solutions

#### 1. AWS Bedrock Access Denied
```bash
# Error: AccessDeniedException when calling the InvokeModel operation
# Solution: Ensure proper IAM permissions
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "bedrock:InvokeModel",
                "bedrock:GetModel"
            ],
            "Resource": "arn:aws:bedrock:*:*:model/anthropic.claude-3-sonnet-20240229-v1:0"
        }
    ]
}
```

#### 2. Audio Recognition Issues
```bash
# Error: Could not understand audio
# Solutions:
# 1. Check microphone permissions
# 2. Reduce background noise
# 3. Speak clearly and at moderate pace
# 4. Check audio input device settings
```

#### 3. PDF Processing Errors
```bash
# Error: Unable to extract text from PDF
# Solutions:
# 1. Ensure PDF is not password protected
# 2. Check if PDF contains searchable text (not just images)
# 3. Try with different PDF files
# 4. Install additional PDF processing libraries: pip install pdfplumber
```

#### 4. Port Already in Use
```bash
# Error: Address already in use
# Solution: Kill existing processes or use different ports
lsof -ti:5000 | xargs kill -9  # Kill process on port 5000
lsof -ti:5001 | xargs kill -9  # Kill process on port 5001
```

### Debug Mode
Enable debug logging by setting environment variables:
```bash
export FLASK_ENV=development
export FLASK_DEBUG=True
export LOG_LEVEL=DEBUG
```

## 📊 Performance Optimization

### Caching Strategies
- **Question Caching**: Store frequently generated questions
- **TTS Caching**: Cache audio files for repeated questions
- **Resume Processing**: Cache extracted skills for similar resumes

### Scalability Considerations
- **Database Integration**: Replace file-based storage with database
- **Load Balancing**: Distribute traffic across multiple instances
- **Background Processing**: Use Celery for heavy AI operations
- **CDN Integration**: Serve static assets from CDN


### Key Performance Indicators
- **Session Completion Rate**: Percentage of completed interviews
- **Question Generation Time**: Average time to generate questions
- **Speech Recognition Accuracy**: Transcription quality metrics
- **User Satisfaction Scores**: Feedback-based ratings

### Monitoring Setup
```python
# Example monitoring integration
import logging
from datetime import datetime

def log_session_metrics(session_id, action, duration=None):
    metrics = {
        'timestamp': datetime.utcnow().isoformat(),
        'session_id': session_id,
        'action': action,
        'duration': duration
    }
    logging.info(f"METRICS: {metrics}")
```

## 📄 License

This project is licensed under the **MIT License** - a permissive open source license that allows for both personal and commercial use.

##  Acknowledgments

- **AWS Bedrock**: AI model infrastructure
- **Anthropic Claude**: Advanced language model capabilities
- **Google TTS**: Text-to-speech services
- **SpeechRecognition**: Audio transcription capabilities
- **Flask**: Web framework foundation

