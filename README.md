# Healthcare Doctor-Patient Translation Web Application

A real-time translation bridge between healthcare providers and patients, featuring AI-powered translation, audio recording, conversation logging, and medical summarization.

## 🎯 Project Overview

This full-stack web application enables seamless communication between doctors and patients who speak different languages. It provides real-time translation, audio recording capabilities, conversation history, and AI-powered medical summaries.

## ✨ Features Implemented

### Core Features (Mandatory)
- ✅ **Real-Time Doctor-Patient Translation**: Role-based messaging with instant translation
- ✅ **Text Chat Interface**: Clean UI with visual distinction between doctor and patient messages
- ✅ **Audio Recording & Storage**: Browser-based recording with playback functionality
- ✅ **Conversation Logging**: Persistent storage with timestamps
- ✅ **Conversation Search**: Keyword search across all conversations with context highlighting
- ✅ **AI-Powered Summary**: Medical summary generation with symptom/diagnosis extraction

### Additional Features
- 🌍 Multi-language support (English, Spanish, Chinese, Hindi, Arabic, French, German)
- 📱 Mobile-responsive design
- 🎨 Modern, healthcare-focused UI
- 💾 Persistent conversation history
- 🔍 Real-time search with highlighting
- 🎙️ Audio waveform visualization

## 🛠️ Tech Stack

### Frontend
- **Framework**: React 18 + Vite
- **Styling**: Tailwind CSS
- **State Management**: React Hooks
- **Audio**: Web Audio API + MediaRecorder API
- **HTTP Client**: Fetch API

### Backend
- **Runtime**: Node.js 18+
- **Framework**: Express.js
- **Database**: SQLite3 (with better-sqlite3)
- **AI Integration**: Anthropic Claude API (Sonnet 4)
- **File Storage**: Local filesystem for audio files

### AI & LLM Integration
- **Translation**: Claude API with custom prompts
- **Summarization**: Claude API with medical context extraction
- **Model**: Claude Sonnet 4 for balance of speed and quality

## 🚀 Quick Start

### Prerequisites
- Node.js 18+ and npm
- Anthropic API key ([Get one here](https://console.anthropic.com/))

### Installation

1. **Clone the repository**
```bash
git clone <your-repo-url>
cd medical-translator
```

2. **Install dependencies**
```bash
# Install backend dependencies
cd backend
npm install

# Install frontend dependencies
cd ../frontend
npm install
```

3. **Configure environment variables**

Create `backend/.env`:
```env
ANTHROPIC_API_KEY=your_api_key_here
PORT=3001
NODE_ENV=development
```

4. **Run the application**

Terminal 1 - Backend:
```bash
cd backend
npm run dev
```

Terminal 2 - Frontend:
```bash
cd frontend
npm run dev
```

5. **Access the application**
- Frontend: http://localhost:5173
- Backend API: http://localhost:3001

## 📁 Project Structure

```
medical-translator/
├── backend/
│   ├── src/
│   │   ├── index.js           # Express server setup
│   │   ├── database.js        # SQLite database initialization
│   │   ├── routes/
│   │   │   ├── conversations.js   # Conversation CRUD endpoints
│   │   │   ├── messages.js        # Message handling & translation
│   │   │   ├── audio.js           # Audio upload/retrieval
│   │   │   └── summary.js         # AI summary generation
│   │   └── services/
│   │       └── claude.js      # Anthropic API integration
│   ├── uploads/               # Audio file storage
│   ├── database.db           # SQLite database
│   └── package.json
├── frontend/
│   ├── src/
│   │   ├── App.jsx           # Main application component
│   │   ├── components/
│   │   │   ├── ChatInterface.jsx      # Main chat UI
│   │   │   ├── MessageBubble.jsx      # Individual messages
│   │   │   ├── AudioRecorder.jsx      # Recording interface
│   │   │   ├── ConversationList.jsx   # Conversation sidebar
│   │   │   ├── SearchBar.jsx          # Search functionality
│   │   │   └── SummaryPanel.jsx       # AI summary display
│   │   ├── services/
│   │   │   └── api.js        # API client
│   │   └── main.jsx
│   ├── index.html
│   └── package.json
└── README.md
```

## 🔑 Key Features Explained

### 1. Real-Time Translation
- Messages are sent with source language and target language
- Claude API translates in real-time preserving medical terminology
- Supports 7+ languages

### 2. Audio Recording
- Browser-based recording using MediaRecorder API
- Audio stored as WebM/MP3 on backend
- Playback directly in conversation thread
- Visual waveform representation

### 3. Conversation Search
- Full-text search across all messages
- Highlights matching keywords
- Shows surrounding context
- Filters by conversation

### 4. AI Medical Summary
- Extracts symptoms, diagnoses, medications, and follow-up actions
- Generates concise bullet-point summaries
- Accessible during or after conversation
- Uses Claude's advanced reasoning capabilities

## 🤖 AI Tools & Resources Used

### Development
- **Claude (Anthropic)**: Code generation, architecture planning, debugging
- **GitHub Copilot**: Code completion and suggestions
- **ChatGPT**: API documentation research

### APIs & Libraries
- **Anthropic Claude API**: Translation and summarization
- **MDN Web Docs**: Web Audio API reference
- **React Documentation**: Component patterns
- **Express.js Docs**: Backend routing

## ⚠️ Known Limitations & Trade-offs

### Current Limitations
1. **Audio Storage**: Files stored locally (not cloud storage like S3)
2. **Real-time**: No WebSocket implementation - uses polling/manual refresh
3. **Authentication**: No user authentication system
4. **Audio Transcription**: Audio not automatically transcribed to text
5. **Scalability**: SQLite suitable for demo but needs PostgreSQL for production

### Trade-off Decisions
- **SQLite over PostgreSQL**: Faster setup, zero configuration, sufficient for demo
- **Local storage over S3**: Reduced complexity, no AWS setup required
- **Polling over WebSockets**: Simpler implementation, easier to deploy
- **Single API key**: No user management, focuses on core features

### Future Enhancements
- [ ] WebSocket for real-time updates
- [ ] User authentication and authorization
- [ ] Audio transcription to text
- [ ] Cloud storage for audio files (AWS S3/Cloudflare R2)
- [ ] PostgreSQL migration for production
- [ ] Export conversation as PDF/email
- [ ] Video call integration
- [ ] Multiple conversation participants

## 🚢 Deployment

### Backend Deployment (Railway/Render)

1. **Prepare for deployment**
   - Ensure `package.json` has start script
   - Add environment variables in platform dashboard
   - Configure PORT variable

2. **Railway**
```bash
# Install Railway CLI
npm i -g @railway/cli

# Login and deploy
railway login
railway init
railway up
```

3. **Render**
   - Connect GitHub repository
   - Select backend directory
   - Add environment variables
   - Deploy

### Frontend Deployment (Vercel)

1. **Update API endpoint**
   - Change `VITE_API_URL` in frontend/.env to deployed backend URL

2. **Deploy**
```bash
# Install Vercel CLI
npm i -g vercel

# Deploy from frontend directory
cd frontend
vercel
```

Or connect GitHub repository to Vercel dashboard.

## 🧪 Testing

### Manual Testing Checklist
- [ ] Send message as doctor, verify patient receives translation
- [ ] Send message as patient, verify doctor receives translation
- [ ] Record audio, verify it appears in conversation
- [ ] Play recorded audio
- [ ] Create new conversation
- [ ] Switch between conversations
- [ ] Search for keywords across conversations
- [ ] Generate AI summary
- [ ] Test on mobile device
- [ ] Test with different languages

### API Testing
```bash
# Test translation endpoint
curl -X POST http://localhost:3001/api/messages \
  -H "Content-Type: application/json" \
  -d '{
    "conversationId": 1,
    "role": "doctor",
    "text": "Hello, how are you feeling?",
    "language": "en",
    "targetLanguage": "es"
  }'
```

## 📄 License

MIT License - feel free to use this project as a reference or starting point.

## 🙏 Acknowledgments

- Anthropic for Claude API
- Nao Medical for the challenge opportunity
- Open source community for amazing libraries

## 📞 Contact

For questions or issues with this submission, please contact through the provided submission form.

---

**Note**: This project was built as a take-home assignment with the goal of demonstrating full-stack capabilities, AI integration, and problem-solving approach within a constrained timeframe. While functional, it represents a foundation that could be extended into a production-ready healthcare application with additional security, scalability, and compliance features.
