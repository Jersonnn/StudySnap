# StudySnap

## Inspiration
As students, we have all experienced the frustration of staring at pages of messy notes the night before an exam, wishing there was a faster way to organize and review everything. Traditional study tools require you to manually create flashcards and quizzes, which takes up valuable time that could be spent actually learning.

We wanted to build something that eliminates that friction. A tool where you simply paste your notes or upload a PDF and instantly get everything you need to study effectively.

The idea behind StudySnap was born from a simple question:
**What if AI could do the tedious part of studying for you, so you can focus on actually learning?**

## What It Does
StudySnap is a mobile study app that takes your raw notes or PDF documents and transforms them into structured learning materials using AI.

When you paste text or upload a PDF, the app generates:

**Reviewer Summaries**
Concise bullet point breakdowns with key terms highlighted.

**Flashcards**
Interactive, swipeable cards for quick memorization.

**Multiple Choice Quizzes**
Auto-generated questions with explanations for each answer.

**Oral Q&A**
Question and answer pairs designed for verbal self-testing with text-to-speech and voice recording capabilities.

**Audio Q&A Mode (NEW!)**
Hands-free study mode with automatic playback:
- Text-to-speech reads questions and answers automatically
- Smart sequence: Question → Wait 3s → Answer → Wait 2s → Next
- Pause/resume controls for flexible studying
- Works even with screen off for true hands-free learning
- Voice recording for self-assessment
- Perfect for commuting, exercising, or multitasking

On top of that, StudySnap includes a **spaced repetition system** that tracks your performance on flashcards using:
- Knew
- Unsure
- Forgot

It then schedules reviews at optimal intervals.

A **streak tracker** keeps you motivated to study every day.

The app works **100% offline** - all your data is stored locally on your device, and only AI generation requires internet.

## How We Built It

### Frontend
- **React Native** with Expo SDK 54
- **expo-router** for file-based navigation
- **react-native-reanimated** for smooth card flip and transition animations
- **AsyncStorage** for local, offline-first data persistence
- **expo-file-system** for PDF handling
- **expo-image-picker** and **expo-document-picker** for file uploads
- **expo-speech** for text-to-speech in Audio Q&A mode
- **expo-av** for audio recording and playback
- **expo-keep-awake** for background audio playback

### AI Integration
- **Google Gemini API** (direct integration, no backend proxy)
- Multiple Gemini models with automatic fallback (Gemini 3, 2.5, 2.0, 1.5)
- Enforced `responseMimeType: "application/json"` for clean structured output
- PDF documents sent directly to AI as base64 for native analysis
- Dynamic prompt generation based on user-specified quantities

### Data Storage
- **Fully offline-first** architecture
- All study sets, flashcards, schedules, and progress stored in AsyncStorage
- No database or server required
- API rate limit tracking from Gemini response headers

### Design
- Custom dark theme with a navy blue palette (#0A1628)
- Inter font family
- Haptic feedback for a polished native feel
- Responsive design that works on all device sizes
- Beautiful onboarding flow with API key setup

## Challenges We Ran Into

### Structured AI Output
Getting Gemini to consistently return valid, well-structured JSON with the correct number of flashcards, quiz questions, and summaries required extensive prompt engineering and response validation. We solved this by:
- Creating dynamic prompts that specify exact quantities
- Implementing robust JSON extraction from various response formats
- Adding multiple model fallback with automatic retry logic

### PDF Direct Analysis
Initially, we tried extracting text from PDFs manually, but this lost formatting and context. We switched to sending PDFs directly to Gemini as base64 inline data, allowing the AI to read and understand the document natively.

### Spaced Repetition Logic
Implementing the scheduling algorithm client-side, with ease factors, interval calculations, and due dates, was mathematically tricky.

The core formula:
```
interval(n+1) = interval(n) × ease
```

Where:
- If the user selects "knew", ease increases
- If the user selects "forgot", ease resets
- If the user selects "unsure", ease adjusts slightly

### Cross-Platform Consistency
Making animations, gestures, and safe area handling work smoothly across iOS, Android, and web required careful platform-specific adjustments and responsive scaling functions.

### Offline-First Architecture
Keeping all data in AsyncStorage while still supporting AI generation required carefully designing the data flow so nothing gets lost. We implemented:
- Automatic data persistence after every change
- Rate limit tracking and display
- Source file storage for regeneration

### Background Audio Playback
Making audio continue playing when the screen is off required:
- Configuring iOS background audio modes
- Using `setInterval` polling instead of `setTimeout` for reliability
- Implementing `expo-keep-awake` to prevent device sleep
- Managing audio session state across app lifecycle

## Accomplishments We're Proud Of

✅ The entire study generation pipeline works end-to-end. Upload a PDF or paste notes and get flashcards, quizzes, summaries, and Q&A in seconds.

✅ The swipeable flashcard UI with flip animations and haptic feedback feels genuinely satisfying to use.

✅ The spaced repetition system intelligently schedules reviews based on user performance.

✅ **AI Lab** feature shows all source materials and allows regeneration with custom quantities.

✅ **Regenerate functionality** on all content types with user-specified counts (e.g., "generate 20 Q&A questions").

✅ The app looks and feels like a polished, market-ready product with its custom dark theme and smooth animations.

✅ **100% offline operation** except for AI generation - no server, no database, no sign-in required.

✅ **Audio Q&A Mode** with automatic playback that works even with screen off for true hands-free studying.

✅ Beautiful onboarding flow that guides users through API key setup.

✅ API usage statistics displayed in Settings showing real-time rate limits.

## What We Learned

### Prompt Engineering Is an Art
Small changes in how you instruct an AI model can dramatically affect output quality and consistency. We learned to:
- Use "EXACTLY X items" instead of "X+ items"
- Provide clear JSON structure examples
- Add quality guidelines and content requirements

### Offline-First Design Requires Discipline
It forces you to think carefully about data flow, persistence, and what truly needs a server. We discovered that most features work better offline.

### Mobile UX Matters
Haptic feedback, smooth animations, and proper safe area handling make the difference between an app that feels amateur and one that feels professional.

### Spaced Repetition Research Is Powerful
Diving into the science behind memory retention and building it into the app gave us a deeper appreciation for evidence-based learning techniques.

### Direct AI Integration Is Better
Instead of building a backend proxy, sending requests directly to Gemini from the mobile app simplified architecture and improved reliability.

### Background Audio Is Tricky
Mobile platforms handle background tasks differently. We learned that:
- `setInterval` is more reliable than `setTimeout` in background
- iOS requires specific background mode permissions
- Keeping the audio session active is crucial for uninterrupted playback

## What's Next for StudySnap

### Enhanced PDF Support
- Multi-page PDF analysis
- PDF annotation and highlighting
- Extract specific pages for focused study

### Cloud Sync (Optional)
- Optional cloud backup for users who want it
- Keep the offline-first approach as default
- End-to-end encryption for privacy

### Social Features
- Share study sets with classmates via QR codes or links
- Collaborate on shared decks
- Study groups and challenges

### Performance Analytics
- Detailed charts showing your study trends
- Identify weak areas and improvement over time
- Study session history and time tracking

### More AI Models
- Support additional providers beyond Gemini
- Let users choose their preferred AI model
- Compare results from different models

### Smart Study Recommendations
- AI suggests which topics to review based on performance
- Optimal study session timing
- Personalized learning paths

---

## Built For
**DeveloperWeek 2026 Hackathon** - Replit Track

## Developer
**Jerson Cerezo** (@jersondev)

## Tech Stack
React Native • Expo • TypeScript • Google Gemini AI • AsyncStorage • Reanimated
- Demo Video: [Video Link]
- Live App: [[StudySnap]](https://drive.google.com/file/d/1vaHwKdQiAcqqRl4H9h3_8OErgDB3nBFi/view?usp=drive_link)
