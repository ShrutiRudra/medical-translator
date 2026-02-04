# Testing Guide

This document provides comprehensive testing procedures for the Medical Translator application.

## Manual Testing Checklist

### ✅ Core Functionality Tests

#### 1. Conversation Management

**Create Conversation**
- [ ] Click "New Conversation" button
- [ ] Enter conversation title
- [ ] Verify conversation appears in sidebar
- [ ] Verify conversation is selected automatically

**View Conversations**
- [ ] Multiple conversations display in sidebar
- [ ] Message count shows correctly
- [ ] Last update time displays
- [ ] Language badges show correctly (EN → ES, etc.)

**Switch Conversations**
- [ ] Click different conversation in sidebar
- [ ] Verify messages load for selected conversation
- [ ] Verify header updates with correct title

**Delete Conversation**
- [ ] Hover over conversation
- [ ] Click delete icon
- [ ] Confirm deletion
- [ ] Verify conversation is removed
- [ ] Verify chat area updates appropriately

#### 2. Message Translation

**Doctor to Patient Translation**
- [ ] Select "Doctor" role
- [ ] Set Doctor language to English
- [ ] Set Patient language to Spanish
- [ ] Type: "How are you feeling today?"
- [ ] Send message
- [ ] Verify message appears in blue bubble
- [ ] Verify translation appears below original text
- [ ] Expected translation: "¿Cómo te sientes hoy?" or similar

**Patient to Doctor Translation**
- [ ] Select "Patient" role
- [ ] Type in Spanish: "Me duele la cabeza"
- [ ] Send message
- [ ] Verify message appears in white bubble
- [ ] Verify translation: "My head hurts" or similar

**Multi-language Support**
- [ ] Test with different language combinations:
  - English ↔ Chinese
  - French ↔ Arabic
  - German ↔ Hindi
- [ ] Verify translations are accurate
- [ ] Verify proper character encoding for non-Latin scripts

**Medical Terminology**
- [ ] Test medical terms are preserved:
  - "hypertension"
  - "diabetes mellitus"
  - "acetaminophen"
- [ ] Verify technical terms remain accurate in translation

#### 3. Audio Recording

**Start Recording**
- [ ] Click microphone button
- [ ] Verify red recording indicator appears
- [ ] Verify timer starts counting
- [ ] Verify audio level visualizer moves with sound

**Stop Recording**
- [ ] Click stop button (red square)
- [ ] Verify recording uploads
- [ ] Verify audio player appears in message
- [ ] Verify message shows "[Audio message]" text

**Play Audio**
- [ ] Click "Play Audio" button on recorded message
- [ ] Verify audio plays back correctly
- [ ] Verify "Playing..." indicator shows
- [ ] Verify playback stops when audio ends

**Audio Permissions**
- [ ] Deny microphone permission
- [ ] Verify appropriate error message
- [ ] Grant permission
- [ ] Verify recording works after granting

#### 4. Search Functionality

**Basic Search**
- [ ] Enter search term in search bar
- [ ] Verify results appear within 500ms
- [ ] Verify matching text is highlighted
- [ ] Verify result count displays

**Search Highlights**
- [ ] Search for "pain"
- [ ] Verify all occurrences are highlighted in yellow
- [ ] Verify context is shown around match

**Clear Search**
- [ ] Click X button in search bar
- [ ] Verify search clears
- [ ] Verify all messages return to view

**No Results**
- [ ] Search for gibberish "xyzabc123"
- [ ] Verify "No results found" message appears

#### 5. AI Summary Generation

**Generate Summary**
- [ ] Have conversation with 5+ messages
- [ ] Click "Summary" button
- [ ] Verify modal opens
- [ ] Click "Generate Summary"
- [ ] Verify loading indicator appears
- [ ] Verify summary generates within 5-10 seconds

**Summary Content**
- [ ] Verify summary includes:
  - Chief Complaint
  - Symptoms
  - Diagnosis (if mentioned)
  - Treatment Plan (if mentioned)
  - Follow-up (if mentioned)

**Regenerate Summary**
- [ ] Click "Regenerate Summary" button
- [ ] Verify new summary generates
- [ ] Verify timestamp updates

**Summary Error Handling**
- [ ] Try with empty conversation
- [ ] Verify appropriate error message
- [ ] Verify option to retry

#### 6. Language Settings

**Change Doctor Language**
- [ ] Click doctor language dropdown
- [ ] Select different language
- [ ] Verify conversation updates
- [ ] Send new message
- [ ] Verify translation uses new language

**Change Patient Language**
- [ ] Click patient language dropdown
- [ ] Select different language
- [ ] Verify conversation updates
- [ ] Send new message
- [ ] Verify translation uses new language

#### 7. Role Switching

**Switch Role Mid-Conversation**
- [ ] Start as Doctor
- [ ] Send message
- [ ] Switch to Patient
- [ ] Verify button visual changes
- [ ] Send message
- [ ] Verify message styling matches role

### ✅ UI/UX Tests

**Responsiveness**
- [ ] Resize browser window to mobile size (375px)
- [ ] Verify sidebar collapses or adapts
- [ ] Verify chat interface remains usable
- [ ] Verify buttons are touchable
- [ ] Test on actual mobile device

**Visual Feedback**
- [ ] Hover over buttons - verify hover states
- [ ] Click buttons - verify active states
- [ ] Verify loading spinners during operations
- [ ] Verify success/error messages appear

**Accessibility**
- [ ] Tab through interface with keyboard
- [ ] Verify focus indicators visible
- [ ] Verify Enter key sends messages
- [ ] Verify Shift+Enter creates new line

**Performance**
- [ ] Load conversation with 50+ messages
- [ ] Verify smooth scrolling
- [ ] Verify no lag when typing
- [ ] Verify search performs quickly

### ✅ Error Handling Tests

**Network Errors**
- [ ] Stop backend server
- [ ] Try sending message
- [ ] Verify appropriate error message
- [ ] Restart backend
- [ ] Verify app recovers

**API Errors**
- [ ] Use invalid API key
- [ ] Try sending message
- [ ] Verify error is caught and displayed
- [ ] Check backend logs for error details

**Invalid Input**
- [ ] Try sending empty message
- [ ] Verify send button is disabled
- [ ] Try sending only whitespace
- [ ] Verify handled appropriately

**File Upload Errors**
- [ ] Record very long audio (2+ minutes)
- [ ] Verify file size limit warning if applicable
- [ ] Try uploading when backend is down
- [ ] Verify error message appears

### ✅ Data Persistence Tests

**Conversation Persistence**
- [ ] Create conversation
- [ ] Send several messages
- [ ] Refresh browser page
- [ ] Verify conversation still exists
- [ ] Verify all messages are preserved

**Audio Persistence**
- [ ] Record and send audio message
- [ ] Refresh page
- [ ] Verify audio is still playable
- [ ] Verify audio file still exists in uploads/

**Settings Persistence**
- [ ] Change language settings
- [ ] Refresh page
- [ ] Select conversation
- [ ] Verify language settings persisted

## API Testing

### Using cURL

**Health Check**
```bash
curl http://localhost:3001/health
```

**Create Conversation**
```bash
curl -X POST http://localhost:3001/api/conversations \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Test Conversation",
    "doctor_language": "en",
    "patient_language": "es"
  }'
```

**Send Message**
```bash
curl -X POST http://localhost:3001/api/messages \
  -H "Content-Type: application/json" \
  -d '{
    "conversationId": 1,
    "role": "doctor",
    "text": "Hello, how can I help you?",
    "language": "en"
  }'
```

**Search Messages**
```bash
curl "http://localhost:3001/api/messages/search?q=pain"
```

**Generate Summary**
```bash
curl -X POST http://localhost:3001/api/summary \
  -H "Content-Type: application/json" \
  -d '{"conversationId": 1}'
```

### Using Browser Console

Open browser DevTools (F12) and run:

```javascript
// Test API connection
fetch('http://localhost:3001/health')
  .then(r => r.json())
  .then(console.log);

// Create conversation
fetch('http://localhost:3001/api/conversations', {
  method: 'POST',
  headers: {'Content-Type': 'application/json'},
  body: JSON.stringify({
    title: 'Console Test',
    doctor_language: 'en',
    patient_language: 'es'
  })
}).then(r => r.json()).then(console.log);
```

## Automated Testing (Future Enhancement)

For production deployment, consider adding:

### Backend Tests (Jest)
```javascript
describe('Translation API', () => {
  it('should translate English to Spanish', async () => {
    const result = await translateText('Hello', 'en', 'es');
    expect(result).toContain('Hola');
  });
});
```

### Frontend Tests (Vitest + React Testing Library)
```javascript
describe('MessageBubble', () => {
  it('should render doctor message correctly', () => {
    render(<MessageBubble message={mockMessage} isDoctor={true} />);
    expect(screen.getByText(mockMessage.original_text)).toBeInTheDocument();
  });
});
```

### End-to-End Tests (Playwright)
```javascript
test('complete conversation flow', async ({ page }) => {
  await page.goto('http://localhost:5173');
  await page.click('text=New Conversation');
  await page.fill('input[placeholder*="Type"]', 'Test message');
  await page.click('button[type="submit"]');
  await expect(page.locator('.message-bubble')).toBeVisible();
});
```

## Performance Benchmarks

Expected performance metrics:

- **Message send**: < 2 seconds (including translation)
- **Page load**: < 3 seconds
- **Search results**: < 500ms
- **Summary generation**: 5-10 seconds
- **Audio upload**: < 3 seconds

## Browser Compatibility

Tested and supported browsers:

- ✅ Chrome 100+
- ✅ Firefox 100+
- ✅ Safari 15+
- ✅ Edge 100+
- ❌ IE 11 (not supported)

## Known Issues

Document any known issues here:

1. **Audio format**: Safari may have issues with WebM format (consider MP3 fallback)
2. **Large files**: Audio files > 10MB may fail to upload
3. **Long conversations**: Performance degrades with 200+ messages (implement pagination)

## Reporting Bugs

When reporting bugs, include:

1. Steps to reproduce
2. Expected behavior
3. Actual behavior
4. Screenshots/videos if applicable
5. Browser and OS version
6. Console errors (F12 → Console)
7. Network errors (F12 → Network)

---

## Quick Test Script

Run through this in 5 minutes for a quick smoke test:

1. ✅ Start backend and frontend
2. ✅ Create new conversation
3. ✅ Send message as doctor (English)
4. ✅ Switch to patient, send message (Spanish)
5. ✅ Record audio message
6. ✅ Play audio back
7. ✅ Search for a word
8. ✅ Generate AI summary
9. ✅ Refresh page, verify persistence
10. ✅ Delete conversation

If all pass, the app is working correctly! 🎉
