# Setup Instructions

Follow these steps to get the Medical Translator application running locally.

## Prerequisites

Before you begin, ensure you have the following installed:

- **Node.js 18+** and npm ([Download](https://nodejs.org/))
- **Git** ([Download](https://git-scm.com/))
- **Anthropic API Key** ([Get one here](https://console.anthropic.com/))

### Verify Installation

```bash
node --version  # Should be 18.0.0 or higher
npm --version   # Should be 9.0.0 or higher
```

## Step-by-Step Setup

### 1. Clone or Download the Repository

If you have a repository:
```bash
git clone <your-repository-url>
cd medical-translator
```

### 2. Backend Setup

```bash
# Navigate to backend directory
cd backend

# Install dependencies
npm install

# Create environment file
cp .env.example .env

# Edit .env file and add your API key
# Open .env in your text editor and replace:
# ANTHROPIC_API_KEY=your_api_key_here
# with your actual API key from https://console.anthropic.com/

# For Windows users using notepad:
notepad .env

# For Mac/Linux users:
nano .env
# or
code .env  # if you have VS Code
```

**Your `.env` file should look like this:**
```env
ANTHROPIC_API_KEY=sk-ant-api03-xxxxxxxxxxxxxxxxxxxxxxxxxxxxx
PORT=3001
NODE_ENV=development
```

### 3. Frontend Setup

Open a new terminal window:

```bash
# Navigate to frontend directory
cd frontend

# Install dependencies
npm install

# Create environment file (optional for local development)
cp .env.example .env
```

### 4. Running the Application

You'll need **two terminal windows** open.

#### Terminal 1 - Backend Server:

```bash
cd backend
npm run dev
```

You should see:
```
✅ Database initialized successfully
🚀 Server running on port 3001
📍 Local: http://localhost:3001
🏥 Medical Translator API ready
```

#### Terminal 2 - Frontend Server:

```bash
cd frontend
npm run dev
```

You should see:
```
  VITE v6.0.5  ready in 500 ms

  ➜  Local:   http://localhost:5173/
  ➜  Network: use --host to expose
```

### 5. Access the Application

Open your web browser and navigate to:
```
http://localhost:5173
```

You should see the Medical Translator interface!

## First Steps

1. **Create a Conversation**
   - Click "New Conversation" in the sidebar
   - Enter a title (e.g., "Test Consultation")

2. **Select Languages**
   - Choose doctor's language (e.g., English)
   - Choose patient's language (e.g., Spanish)

3. **Send a Message**
   - Select your role (Doctor or Patient)
   - Type a message
   - Press Enter or click Send
   - Watch it get translated in real-time!

4. **Try Audio Recording**
   - Click the microphone button
   - Speak your message
   - Click the stop button
   - The audio will be uploaded and attached

5. **Generate Summary**
   - After a few messages, click "Summary" button
   - Wait for AI to analyze the conversation
   - View the medical summary

## Troubleshooting

### Backend won't start

**Error**: `ANTHROPIC_API_KEY not set`
- Solution: Make sure you created `.env` file and added your API key

**Error**: `Port 3001 is already in use`
- Solution: Kill the process using port 3001:
  ```bash
  # On Mac/Linux:
  lsof -ti:3001 | xargs kill -9
  
  # On Windows:
  netstat -ano | findstr :3001
  taskkill /PID <PID_NUMBER> /F
  ```

**Error**: `Cannot find module 'better-sqlite3'`
- Solution: Delete `node_modules` and reinstall:
  ```bash
  rm -rf node_modules package-lock.json
  npm install
  ```

### Frontend won't start

**Error**: `Failed to resolve import`
- Solution: Clear cache and reinstall:
  ```bash
  rm -rf node_modules .vite package-lock.json
  npm install
  ```

**Error**: `Cannot connect to backend`
- Solution: Make sure backend is running on port 3001
- Check browser console for CORS errors

### Translation not working

**Error**: Messages send but don't translate
- Check backend logs for API errors
- Verify your API key is valid
- Check API key has sufficient credits

### Audio recording not working

**Error**: "Could not access microphone"
- Grant microphone permissions in your browser
- Check browser console for errors
- Try a different browser (Chrome/Edge recommended)

## Development Tips

### Viewing API Requests

Backend logs all requests to console. Watch Terminal 1 to see:
- Incoming API calls
- Translation requests
- Database queries
- Errors

### Database Management

The SQLite database is stored at `backend/database.db`

To view database contents:
```bash
cd backend
sqlite3 database.db
# In SQLite prompt:
.tables              # List tables
SELECT * FROM conversations;
SELECT * FROM messages;
.quit                # Exit
```

To reset database:
```bash
rm backend/database.db
# Restart backend - it will recreate the database
```

### Hot Reloading

Both frontend and backend support hot reloading:
- **Backend**: Uses Node's `--watch` flag (Node 18+)
- **Frontend**: Vite automatically reloads on file changes

### Checking Backend Health

```bash
curl http://localhost:3001/health
```

Should return:
```json
{
  "status": "healthy",
  "timestamp": "2024-01-15T10:30:00.000Z",
  "uptime": 123.456
}
```

## Next Steps

Once everything is working:

1. **Read the full README.md** for feature documentation
2. **Check DEPLOYMENT.md** for deployment instructions
3. **Explore the code** to understand the architecture
4. **Make improvements** and add features!

## Getting Help

If you're stuck:

1. Check the error message carefully
2. Look for solutions in this troubleshooting section
3. Check browser console (F12) for frontend errors
4. Check terminal for backend errors
5. Verify all dependencies are installed correctly

## Common Questions

**Q: Do I need to pay for the Anthropic API?**
A: Yes, but you get free credits when you sign up. The cost is very low for development/testing.

**Q: Can I use a different AI model?**
A: Yes! Edit `backend/src/services/claude.js` to use a different model or provider.

**Q: How do I add more languages?**
A: Edit the `LANGUAGES` array in `frontend/src/components/ChatInterface.jsx`

**Q: Can I deploy this for free?**
A: Yes! Use free tiers of Railway/Vercel. See DEPLOYMENT.md for details.

**Q: Is this HIPAA compliant?**
A: No, this is a demo application. For production healthcare use, you'd need additional security measures.

## Project Structure Overview

```
medical-translator/
├── backend/              # Node.js/Express API
│   ├── src/
│   │   ├── index.js      # Server entry point
│   │   ├── database.js   # SQLite setup
│   │   ├── routes/       # API endpoints
│   │   └── services/     # Claude API integration
│   └── uploads/          # Audio file storage
├── frontend/             # React/Vite app
│   ├── src/
│   │   ├── App.jsx       # Main app component
│   │   ├── components/   # UI components
│   │   └── services/     # API client
│   └── public/           # Static assets
└── README.md             # Main documentation
```

---

Happy coding! 🏥💬
