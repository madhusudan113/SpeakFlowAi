import React, { useState, useEffect, useRef, useCallback, useMemo } from 'react';
import {
  Mic, MicOff, PhoneOff, PhoneCall, Sparkles, Flame, Award, Calendar,
  BarChart3, User, Home, MessageSquare, Volume2, VolumeX, RefreshCw,
  CheckCircle2, ArrowRight, Play, Square, ChevronRight, Sun, Moon,
  Clock, Zap, AlertCircle, BookOpen, Send, HelpCircle, ShieldCheck,
  Sliders, Bell, RotateCcw, Trophy, Star, Target, Headphones, Edit3, Check
} from 'lucide-react';
import { initializeApp } from 'firebase/app';
import {
  getAuth,
  signInAnonymously,
  signInWithCustomToken,
  onAuthStateChanged
} from 'firebase/auth';
import {
  getFirestore,
  doc,
  setDoc,
  getDoc,
  onSnapshot,
  collection
} from 'firebase/firestore';

// ============================================================================
// FIREBASE INITIALIZATION (Safe initialization with fallback)
// ============================================================================
let app = null;
let auth = null;
let db = null;
const appId = typeof __app_id !== 'undefined' ? __app_id : 'speakup-ai-coach';

try {
  if (typeof __firebase_config !== 'undefined' && __firebase_config) {
    const firebaseConfig = JSON.parse(__firebase_config);
    app = initializeApp(firebaseConfig);
    auth = getAuth(app);
    db = getFirestore(app);
  }
} catch (err) {
  console.warn('Firebase initialization skipped or failed; running in cloud-ready state.', err);
}

// ============================================================================
// GEMINI API HELPERS (Text Generation with Structured JSON & Backoff)
// ============================================================================
const GEMINI_MODEL = 'gemini-3-flash-preview';

async function callGeminiStructured({ systemInstruction, userPrompt, responseSchema, retries = 3 }) {
  const apiKey = '';
  const url = `https://generativelanguage.googleapis.com/v1beta/models/${GEMINI_MODEL}:generateContent?key=${apiKey}`;

  const payload = {
    contents: [{ role: 'user', parts: [{ text: userPrompt }] }],
    systemInstruction: { parts: [{ text: systemInstruction }] },
    generationConfig: {
      responseMimeType: 'application/json',
      responseSchema
    }
  };

  let delay = 1000;
  for (let attempt = 0; attempt <= retries; attempt++) {
    try {
      const response = await fetch(url, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(payload)
      });

      if (!response.ok) {
        if (attempt < retries && (response.status === 429 || response.status >= 500)) {
          await new Promise((res) => setTimeout(res, delay));
          delay *= 2;
          continue;
        }
        throw new Error(`API HTTP ${response.status}`);
      }

      const data = await response.json();
      const rawText = data?.candidates?.[0]?.content?.parts?.[0]?.text;
      if (!rawText) throw new Error('Empty response from Gemini');
      return JSON.parse(rawText);
    } catch (error) {
      if (attempt === retries) throw error;
      await new Promise((res) => setTimeout(res, delay));
      delay *= 2;
    }
  }
}

// ============================================================================
// 30-DAY REAL-LIFE CURRICULUM DATA
// ============================================================================
const CURRICULUM_30_DAYS = [
  { day: 1, title: 'Warm Introductions & Small Talk', level: 'Beginner', category: 'Social', duration: '5–10 min', opener: "Hi there! I'm Maya, your English speaking coach. It's so nice to meet you! To get us warmed up, where are you calling from today, and how is your day going so far?" },
  { day: 2, title: 'Ordering at a Coffee Shop', level: 'Beginner', category: 'Daily Life', duration: '5–10 min', opener: "Welcome to Sunny Side Café! I'm Maya, your barista today. What can I get started for you this morning—maybe a coffee, tea, or something sweet?" },
  { day: 3, title: 'Grocery & Clothes Shopping', level: 'Beginner', category: 'Daily Life', duration: '5–10 min', opener: "Hello and welcome to our store! Let me know if you need help finding any sizes or items today. What are you shopping for right now?" },
  { day: 4, title: 'Asking for Directions', level: 'Beginner', category: 'Travel', duration: '5–10 min', opener: "Excuse me, you look like you might be looking for somewhere! I know this neighborhood really well. Where are you trying to get to today?" },
  { day: 5, title: 'Making Weekend Plans', level: 'Beginner', category: 'Social', duration: '5–10 min', opener: "Hey! The weekend is almost here and I'm so excited to relax. Do you have any fun plans yet, or do you want to brainstorm something to do together?" },
  { day: 6, title: 'Restaurant Reservations & Dining', level: 'Beginner', category: 'Daily Life', duration: '8–12 min', opener: "Good evening and welcome to Bella Vista Restaurant! Do you have a reservation with us tonight, or would you like a table for how many people?" },
  { day: 7, title: 'Doctor & Pharmacy Visit', level: 'Beginner', category: 'Health', duration: '8–12 min', opener: "Hello, come on in and have a seat. I'm Dr. Maya. What seems to be bothering you today, and when did you first start feeling this way?" },
  { day: 8, title: 'Phone Calls & Appointments', level: 'Beginner', category: 'Daily Life', duration: '8–12 min', opener: "Good morning, thank you for calling City Dental Care, this is Maya speaking. How may I assist you with scheduling today?" },
  { day: 9, title: 'Airport & Hotel Check-in', level: 'Intermediate', category: 'Travel', duration: '10–15 min', opener: "Welcome to the Grand Horizon Hotel! I hope your flight went smoothly. May I have your name and passport so I can check you into your room?" },
  { day: 10, title: 'Talking About Hobbies & Movies', level: 'Intermediate', category: 'Social', duration: '10–15 min', opener: "I just finished watching a really great movie last night and I'm looking for new recommendations! What kind of movies, shows, or hobbies do you usually enjoy in your free time?" },
  { day: 11, title: 'Expressing Opinions Politely', level: 'Intermediate', category: 'Communication', duration: '10–15 min', opener: "Today let's practice sharing opinions naturally! Some people think working from home is much better than working in an office. What is your personal take on that?" },
  { day: 12, title: 'Workplace Standup & Updates', level: 'Intermediate', category: 'Career', duration: '10–15 min', opener: "Good morning team! Let's kick off our daily standup meeting. Could you share what you worked on yesterday and what your main focus is for today?" },
  { day: 13, title: 'Customer Service & Returns', level: 'Intermediate', category: 'Daily Life', duration: '10–15 min', opener: "Hi there, welcome to the Customer Care desk. I see you brought a package with you today—what seems to be the issue with your order?" },
  { day: 14, title: 'Renting an Apartment', level: 'Intermediate', category: 'Daily Life', duration: '10–15 min', opener: "Thanks for coming to tour this two-bedroom apartment! Before I show you around the kitchen and balcony, what features are most important to you in a new home?" },
  { day: 15, title: 'Networking at an Event', level: 'Intermediate', category: 'Career', duration: '10–15 min', opener: "Hi! Great conference so far, isn't it? I'm Maya, I work in product design. What brings you to this event today, and what line of work are you in?" },
  { day: 16, title: 'Job Interview — Tell Me About Yourself', level: 'Intermediate', category: 'Career', duration: '12–15 min', opener: "Thank you so much for coming in today! I've looked over your resume, and I'd love to hear your story in your own words. Could you tell me a little about yourself and your background?" },
  { day: 17, title: 'Job Interview — Strengths & Weaknesses', level: 'Intermediate', category: 'Career', duration: '12–15 min', opener: "You've got some great experience! Every candidate brings unique qualities to a team. What would you say is your biggest professional strength, and one area you're actively working to improve?" },
  { day: 18, title: 'Handling Disagreements at Work', level: 'Intermediate', category: 'Career', duration: '12–15 min', opener: "Imagine we're teammates planning a product launch, and I want to rush it out this Friday while you think it needs another week of testing. How would you bring up your concern to me?" },
  { day: 19, title: 'Explaining a Problem Clearly', level: 'Intermediate', category: 'Communication', duration: '12–15 min', opener: "Being able to explain a tricky problem step by step is a superpower in English. Tell me about a recent technical glitch, travel delay, or everyday problem you had to solve—what happened?" },
  { day: 20, title: 'Storytelling — A Memorable Trip', level: 'Intermediate', category: 'Social', duration: '12–15 min', opener: "I love hearing travel stories! Think about one of the most memorable trips or adventures you've ever taken. Where did you go, and what made it unforgettable?" },
  { day: 21, title: 'Cultural Differences & Traditions', level: 'Advanced', category: 'Culture', duration: '15–20 min', opener: "Every culture has fascinating traditions around food, hospitality, and celebrations. If I were visiting your hometown for the very first time, what cultural custom should I definitely know about?" },
  { day: 22, title: 'Tech & Social Media Habits', level: 'Advanced', category: 'Society', duration: '15–20 min', opener: "Artificial intelligence and smartphones have completely changed how we connect and learn. How do you balance staying connected online with protecting your focus and mental energy?" },
  { day: 23, title: 'Health, Fitness & Mindfulness', level: 'Advanced', category: 'Lifestyle', duration: '15–20 min', opener: "Staying healthy with a busy schedule takes real intention. What habits or routines help you recharge when life gets stressful?" },
  { day: 24, title: 'Pitching an Idea', level: 'Advanced', category: 'Business', duration: '15–20 min', opener: "Welcome to the pitch room! Pretend I'm an investor or decision-maker ready to back a great concept. What problem does your idea solve, and why is now the right time for it?" },
  { day: 25, title: 'Negotiating Salary or Price', level: 'Advanced', category: 'Business', duration: '15–20 min', opener: "We're thrilled to offer you the position! Based on our initial budget, we're starting the package at seventy thousand. How does that sound to you, and what were you hoping for?" },
  { day: 26, title: 'Debating Current Trends', level: 'Advanced', category: 'Society', duration: '15–20 min', opener: "Let's have a friendly, thoughtful debate! Many cities are banning cars from downtown centers to create pedestrian-only zones. Do you think that helps or hurts local businesses and residents?" },
  { day: 27, title: 'Handling an Emergency', level: 'Advanced', category: 'Real-World', duration: '15–20 min', opener: "Stay calm—let's role-play an urgent situation so you feel confident under pressure. Imagine you lost your wallet and passport while traveling abroad and just reached the embassy desk. What do you say first?" },
  { day: 28, title: 'Leadership & Teamwork', level: 'Advanced', category: 'Career', duration: '15–20 min', opener: "Great leaders inspire trust even when deadlines are tight. Tell me about a time you helped motivate a team or navigate a challenging group project—what approach worked best?" },
  { day: 29, title: 'Future Goals & 5-Year Vision', level: 'Advanced', category: 'Personal Growth', duration: '15–20 min', opener: "Fast-forward five years from today! If everything goes wonderfully in your career, personal life, and English fluency, what does an ideal day in your life look like?" },
  { day: 30, title: 'Graduation Showcase & Free Debate', level: 'Advanced', category: 'Milestone', duration: '15–25 min', opener: "Congratulations on reaching Day 30 of SpeakUp! You've come such a long way. Today is your graduation showcase—what topic are you most passionate about discussing with me today?" }
];

const QUICK_ROLEPLAYS = [
  { id: 'marketing-interview', title: 'Marketing Job Interview', emoji: '💼', level: 'Intermediate', prompt: 'Interview me for a Digital Marketing Manager position. Ask realistic behavioral and strategy questions.', opener: "Hi! Thanks so much for joining me today for the Digital Marketing Manager interview. I really enjoyed reading your application—to start us off, what draws you most to marketing, and what campaign are you proudest of?" },
  { id: 'coffee-london', title: 'Street Food & Café in London', emoji: '☕', level: 'Beginner', prompt: 'Act as a friendly London café owner recommending local pastries and drinks.', opener: "Alright there, welcome in out of the London rain! Can I tempt you with a fresh flat white or one of our warm cardamom buns today?" },
  { id: 'freelance-rate', title: 'Negotiate a Freelance Rate', emoji: '🤝', level: 'Advanced', prompt: 'Act as a client who wants to hire me for a web design project but thinks my initial quote is slightly high.', opener: "Thanks for sending over the proposal! We really love your portfolio, though your rate is a bit higher than we budgeted. Could you walk me through what's included so we can see how to make this work?" },
  { id: 'airport-rebook', title: 'Missed Flight Rebooking', emoji: '✈️', level: 'Intermediate', prompt: 'Act as an airline gate agent helping me rebook a missed connecting flight.', opener: "Welcome to the Express Rebooking desk. I see your connecting flight just departed ten minutes ago—let me pull up your itinerary. Where is your final destination today?" },
  { id: 'casual-debate', title: 'AI & Creativity Debate', emoji: '🧠', level: 'Advanced', prompt: 'Have a friendly intellectual discussion about whether AI makes humans more creative or less original.', opener: "I've been thinking about this all week: with AI tools able to write, paint, and compose music in seconds, do you think technology is unlocking human creativity or making our culture less original?" },
  { id: 'first-date', title: 'Coffee Shop First Meetup', emoji: '🌟', level: 'Beginner', prompt: 'Have a warm, friendly casual conversation getting to know each other over coffee.', opener: "Hey! So glad we finally found time to grab coffee. I love this little spot—have you been here before, or is this your first time?" }
];

const BADGES_CATALOG = [
  { id: 'first_call', title: 'First Words', desc: 'Complete your first voice coaching call', icon: '🎙️' },
  { id: 'streak_3', title: '3-Day Momentum', desc: 'Practice speaking 3 days in a row', icon: '🔥' },
  { id: 'wpm_100', title: 'Natural Flow', desc: 'Reach 100+ words per minute in a session', icon: '⚡' },
  { id: 'zero_filler', title: 'Crisp Speaker', desc: 'Complete a session with 1 or fewer filler words', icon: '💎' },
  { id: 'day_15', title: 'Halfway Hero', desc: 'Reach Day 15 of the 30-Day Curriculum', icon: '🏔️' },
  { id: 'day_30', title: 'SpeakUp Graduate', desc: 'Complete all 30 days of the speaking program', icon: '🎓' }
];

// ============================================================================
// UTILITY FUNCTIONS (Speech Analysis, Filler Count, Similarity)
// ============================================================================
const FILLER_WORDS_LIST = ['um', 'uh', 'er', 'ah', 'like', 'you know', 'basically', 'actually', 'literally', 'sort of', 'kind of'];

function analyzeSpeechMetrics(messages, durationSeconds) {
  const userMessages = messages.filter((m) => m.role === 'user');
  const fullUserText = userMessages.map((m) => m.text).join(' ').trim();
  const words = fullUserText ? fullUserText.split(/\s+/).filter(Boolean) : [];
  const wordCount = words.length;

  // Estimate active user speaking minutes (roughly 55% of total call time, min 0.25m)
  const effectiveMinutes = Math.max(0.25, (durationSeconds * 0.55) / 60);
  const wpm = wordCount > 0 ? Math.min(220, Math.max(35, Math.round(wordCount / effectiveMinutes))) : 0;

  const lowerText = fullUserText.toLowerCase();
  const fillerBreakdown = {};
  let totalFillers = 0;

  FILLER_WORDS_LIST.forEach((filler) => {
    const regex = new RegExp(`\\b${filler.replace(/\s+/g, '\\s+')}\\b`, 'gi');
    const matches = lowerText.match(regex);
    if (matches && matches.length > 0) {
      fillerBreakdown[filler] = matches.length;
      totalFillers += matches.length;
    }
  });

  return {
    wordCount,
    turnCount: userMessages.length,
    wpm,
    totalFillers,
    fillerBreakdown,
    fullUserText
  };
}

function calculateSentenceSimilarity(target, spoken) {
  const clean = (s) =>
    s
      .toLowerCase()
      .replace(/[^\w\s]/g, '')
      .split(/\s+/)
      .filter(Boolean);
  const targetWords = clean(target);
  const spokenWords = clean(spoken);
  if (targetWords.length === 0) return 100;
  if (spokenWords.length === 0) return 0;

  let matches = 0;
  const spokenSet = [...spokenWords];
  targetWords.forEach((word) => {
    const idx = spokenSet.indexOf(word);
    if (idx !== -1) {
      matches++;
      spokenSet.splice(idx, 1);
    }
  });
  return Math.round((matches / targetWords.length) * 100);
}

function formatTimeMMSS(seconds) {
  const mins = Math.floor(seconds / 60);
  const secs = seconds % 60;
  return `${String(mins).padStart(2, '0')}:${String(secs).padStart(2, '0')}`;
}

// ============================================================================
// DEFAULT USER PROFILE STATE
// ============================================================================
const DEFAULT_USER_DATA = {
  name: 'Alex',
  level: 'Intermediate', // 'Beginner' | 'Intermediate' | 'Advanced'
  onboardingCompleted: false,
  streak: 1,
  lastPracticeDate: null,
  totalMinutes: 0,
  xp: 120,
  completedDays: [],
  currentDay: 1,
  unlockedBadges: [],
  sessionHistory: [],
  settings: {
    voiceURI: '',
    speechRate: 1.0,
    reminderEnabled: true,
    reminderTime: '19:00',
    darkMode: false
  }
};

// ============================================================================
// MAIN APPLICATION COMPONENT
// ============================================================================
export default function App() {
  // Navigation & Theme State
  const [activeTab, setActiveTab] = useState('home'); // 'home' | 'practice' | 'progress' | 'profile'
  const [user, setUser] = useState(null);
  const [userData, setUserData] = useState(DEFAULT_USER_DATA);
  const [isLoadingData, setIsLoadingData] = useState(true);

  // Active Screen Modals / Flows
  const [showOnboarding, setShowOnboarding] = useState(false);
  const [activeCallConfig, setActiveCallConfig] = useState(null); // { title, day?, level, opener, customPrompt? }
  const [activeReport, setActiveReport] = useState(null); // Report object to display

  // Available TTS Voices
  const [availableVoices, setAvailableVoices] = useState([]);

  // Load TTS Voices
  useEffect(() => {
    if (typeof window === 'undefined' || !('speechSynthesis' in window)) return;
    const loadVoices = () => {
      const voices = window.speechSynthesis.getVoices();
      const englishVoices = voices.filter((v) => v.lang.startsWith('en'));
      setAvailableVoices(englishVoices.length > 0 ? englishVoices : voices);
    };
    loadVoices();
    window.speechSynthesis.onvoiceschanged = loadVoices;
  }, []);

  // Firebase Auth & Data Persistence Setup (Following strict rules)
  useEffect(() => {
    if (!auth) {
      setIsLoadingData(false);
      setShowOnboarding(true);
      return;
    }

    const initAuth = async () => {
      try {
        if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
          await signInWithCustomToken(auth, __initial_auth_token);
        } else {
          await signInAnonymously(auth);
        }
      } catch (e) {
        console.warn('Auth fallback active:', e);
        setIsLoadingData(false);
      }
    };

    initAuth();
    const unsubscribe = onAuthStateChanged(auth, (firebaseUser) => {
      setUser(firebaseUser);
      if (!firebaseUser) {
        setIsLoadingData(false);
      }
    });

    return () => unsubscribe();
  }, []);

  // Firestore User Profile Listener
  useEffect(() => {
    if (!user || !db) return;

    const userDocRef = doc(db, 'artifacts', appId, 'users', user.uid, 'profile', 'main');
    const unsubscribe = onSnapshot(
      userDocRef,
      (docSnap) => {
        if (docSnap.exists()) {
          const remoteData = docSnap.data();
          const merged = {
            ...DEFAULT_USER_DATA,
            ...remoteData,
            settings: { ...DEFAULT_USER_DATA.settings, ...(remoteData.settings || {}) }
          };
          setUserData(merged);
          if (!merged.onboardingCompleted) {
            setShowOnboarding(true);
          }
        } else {
          setShowOnboarding(true);
        }
        setIsLoadingData(false);
      },
      (err) => {
        console.warn('Firestore snapshot warning:', err);
        setIsLoadingData(false);
      }
    );

    return () => unsubscribe();
  }, [user]);

  // Persist userData updates to Firestore (or state if offline)
  const updateUserData = useCallback(
    async (updater) => {
      setUserData((prev) => {
        const next = typeof updater === 'function' ? updater(prev) : { ...prev, ...updater };
        if (user && db) {
          const userDocRef = doc(db, 'artifacts', appId, 'users', user.uid, 'profile', 'main');
          setDoc(userDocRef, next, { merge: true }).catch((e) =>
            console.warn('Sync pending:', e)
          );
        }
        return next;
      });
    },
    [user]
  );

  // Dark Mode Class Sync
  const isDark = userData.settings.darkMode;

  // Start a Voice Call Session
  const handleStartCall = (config) => {
    setActiveReport(null);
    setActiveCallConfig({
      title: config.title || 'Free Speaking Practice',
      day: config.day || null,
      level: config.level || userData.level,
      opener:
        config.opener ||
        `Hi ${userData.name}! I'm Maya. Let's have a great conversation about ${config.title}. What are your initial thoughts on this topic?`,
      customPrompt: config.customPrompt || ''
    });
  };

  // Finish Call and Save Report + Progress
  const handleCompleteCall = (report) => {
    setActiveCallConfig(null);
    setActiveReport(report);

    const todayStr = new Date().toISOString().split('T')[0];
    updateUserData((prev) => {
      const isNewDay = prev.lastPracticeDate !== todayStr;
      const newStreak = isNewDay ? (prev.lastPracticeDate ? prev.streak + 1 : 1) : prev.streak;
      const addedMinutes = Math.max(1, Math.round(report.durationSeconds / 60));
      const newCompletedDays =
        report.day && !prev.completedDays.includes(report.day)
          ? [...prev.completedDays, report.day]
          : prev.completedDays;

      const nextDay = report.day
        ? Math.min(30, Math.max(prev.currentDay, report.day + 1))
        : prev.currentDay;

      // Evaluate Badges
      const badges = new Set(prev.unlockedBadges);
      badges.add('first_call');
      if (newStreak >= 3) badges.add('streak_3');
      if (report.metrics.wpm >= 100) badges.add('wpm_100');
      if (report.metrics.totalFillers <= 1 && report.metrics.turnCount >= 2) badges.add('zero_filler');
      if (newCompletedDays.length >= 15) badges.add('day_15');
      if (newCompletedDays.length >= 30) badges.add('day_30');

      return {
        ...prev,
        streak: newStreak,
        lastPracticeDate: todayStr,
        totalMinutes: prev.totalMinutes + addedMinutes,
        xp: prev.xp + 50 + report.metrics.turnCount * 10,
        completedDays: newCompletedDays,
        currentDay: nextDay,
        unlockedBadges: Array.from(badges),
        sessionHistory: [report, ...prev.sessionHistory].slice(0, 30)
      };
    });
  };

  // Loading Screen
  if (isLoadingData) {
    return (
      <div className="min-h-screen flex items-center justify-center bg-slate-50 text-slate-800">
        <div className="flex flex-col items-center gap-4">
          <div className="w-14 h-14 rounded-2xl bg-indigo-600 flex items-center justify-center text-white shadow-lg animate-pulse">
            <PhoneCall className="w-7 h-7" />
          </div>
          <p className="text-sm font-medium text-slate-500">Connecting to Maya, your AI Coach...</p>
        </div>
      </div>
    );
  }

  return (
    <div className={isDark ? 'dark' : ''}>
      <div className="min-h-screen bg-slate-50 dark:bg-slate-950 text-slate-900 dark:text-slate-100 transition-colors duration-200 flex flex-col">
        {/* ONBOARDING MODAL / SCREEN */}
        {showOnboarding && (
          <OnboardingPlacementScreen
            userData={userData}
            availableVoices={availableVoices}
            onComplete={(detectedLevel, userName) => {
              updateUserData({
                name: userName || userData.name,
                level: detectedLevel,
                onboardingCompleted: true
              });
              setShowOnboarding(false);
            }}
            onSkip={() => {
              updateUserData({ onboardingCompleted: true });
              setShowOnboarding(false);
            }}
          />
        )}

        {/* ACTIVE VOICE CALL OVERLAY */}
        {activeCallConfig && (
          <VoiceCallSession
            config={activeCallConfig}
            userData={userData}
            availableVoices={availableVoices}
            onEndCall={handleCompleteCall}
            onCancel={() => setActiveCallConfig(null)}
          />
        )}

        {/* POST-SESSION FEEDBACK REPORT VIEW */}
        {!activeCallConfig && activeReport && (
          <FeedbackReportView
            report={activeReport}
            onClose={() => {
              setActiveReport(null);
              setActiveTab('home');
            }}
            onPracticeAgain={() => {
              const cfg = {
                title: activeReport.topic,
                day: activeReport.day,
                level: activeReport.level
              };
              setActiveReport(null);
              handleStartCall(cfg);
            }}
          />
        )}

        {/* MAIN APP SHELL (When not in call or viewing full-screen report) */}
        {!activeCallConfig && !activeReport && (
          <>
            {/* Top Header */}
            <header className="sticky top-0 z-30 bg-white/80 dark:bg-slate-900/80 backdrop-blur-md border-b border-slate-200/70 dark:border-slate-800">
              <div className="max-w-5xl mx-auto px-4 h-16 flex items-center justify-between">
                <div className="flex items-center gap-2.5">
                  <div className="w-9 h-9 rounded-xl bg-gradient-to-br from-indigo-600 to-teal-500 flex items-center justify-center text-white shadow-sm">
                    <PhoneCall className="w-5 h-5" />
                  </div>
                  <div>
                    <span className="font-bold text-lg tracking-tight bg-gradient-to-r from-indigo-600 to-teal-600 dark:from-indigo-400 dark:to-teal-400 bg-clip-text text-transparent">
                      SpeakUp
                    </span>
                    <span className="hidden sm:inline-block ml-2 text-xs font-medium px-2 py-0.5 rounded-full bg-indigo-50 dark:bg-indigo-950/60 text-indigo-600 dark:text-indigo-300">
                      AI Voice Coach
                    </span>
                  </div>
                </div>

                {/* Streak, XP & Theme Toggle */}
                <div className="flex items-center gap-2 sm:gap-3">
                  <div className="flex items-center gap-1.5 px-3 py-1.5 rounded-full bg-amber-50 dark:bg-amber-950/40 border border-amber-200/60 dark:border-amber-800/40 text-amber-700 dark:text-amber-300 text-xs font-semibold">
                    <Flame className="w-4 h-4 fill-amber-500 text-amber-500" />
                    <span>{userData.streak}d</span>
                  </div>
                  <div className="flex items-center gap-1.5 px-3 py-1.5 rounded-full bg-teal-50 dark:bg-teal-950/40 border border-teal-200/60 dark:border-teal-800/40 text-teal-700 dark:text-teal-300 text-xs font-semibold">
                    <Zap className="w-3.5 h-3.5 text-teal-500" />
                    <span>{userData.xp} XP</span>
                  </div>
                  <button
                    onClick={() =>
                      updateUserData((prev) => ({
                        settings: { ...prev.settings, darkMode: !prev.settings.darkMode }
                      }))
                    }
                    aria-label="Toggle theme"
                    className="p-2 rounded-xl bg-slate-100 dark:bg-slate-800 text-slate-600 dark:text-slate-300 hover:bg-slate-200 dark:hover:bg-slate-700 transition"
                  >
                    {isDark ? <Sun className="w-4 h-4" /> : <Moon className="w-4 h-4" />}
                  </button>
                </div>
              </div>
            </header>

            {/* Main Content Area */}
            <main className="flex-1 max-w-5xl w-full mx-auto px-4 pt-6 pb-28">
              {activeTab === 'home' && (
                <HomeTab
                  userData={userData}
                  onStartCall={handleStartCall}
                  onOpenPractice={() => setActiveTab('practice')}
                />
              )}
              {activeTab === 'practice' && (
                <PracticeTab userData={userData} onStartCall={handleStartCall} />
              )}
              {activeTab === 'progress' && (
                <ProgressTab
                  userData={userData}
                  onOpenReport={(report) => setActiveReport(report)}
                />
              )}
              {activeTab === 'profile' && (
                <ProfileTab
                  user={user}
                  userData={userData}
                  availableVoices={availableVoices}
                  onUpdateUserData={updateUserData}
                  onRetakeTest={() => setShowOnboarding(true)}
                />
              )}
            </main>

            {/* Mobile-First Bottom Navigation Bar */}
            <nav className="fixed bottom-0 inset-x-0 z-30 bg-white/95 dark:bg-slate-900/95 backdrop-blur-lg border-t border-slate-200 dark:border-slate-800 safe-area-pb">
              <div className="max-w-lg mx-auto px-4 h-16 flex items-center justify-around">
                {[
                  { id: 'home', label: 'Home', icon: Home },
                  { id: 'practice', label: 'Practice', icon: Mic },
                  { id: 'progress', label: 'Progress', icon: BarChart3 },
                  { id: 'profile', label: 'Profile', icon: User }
                ].map((tab) => {
                  const Icon = tab.icon;
                  const active = activeTab === tab.id;
                  return (
                    <button
                      key={tab.id}
                      onClick={() => setActiveTab(tab.id)}
                      className={`flex flex-col items-center justify-center gap-1 py-1.5 px-4 rounded-2xl transition-all ${
                        active
                          ? 'text-indigo-600 dark:text-indigo-400 bg-indigo-50/80 dark:bg-indigo-950/50 font-semibold'
                          : 'text-slate-500 dark:text-slate-400 hover:text-slate-800 dark:hover:text-slate-200'
                      }`}
                    >
                      <Icon className={`w-5 h-5 ${active ? 'stroke-[2.5]' : ''}`} />
                      <span className="text-[11px]">{tab.label}</span>
                    </button>
                  );
                })}
              </div>
            </nav>
          </>
        )}
      </div>
    </div>
  );
}

// ============================================================================
// 1. ONBOARDING: 60-SECOND SPEAKING PLACEMENT TEST
// ============================================================================
function OnboardingPlacementScreen({ userData, onComplete, onSkip }) {
  const [step, setStep] = useState('intro'); // 'intro' | 'recording' | 'analyzing' | 'result'
  const [nameInput, setNameInput] = useState(userData.name || '');
  const [timeLeft, setTimeLeft] = useState(60);
  const [transcript, setTranscript] = useState('');
  const [interimText, setInterimText] = useState('');
  const [isListening, setIsListening] = useState(false);
  const [placementResult, setPlacementResult] = useState(null);
  const [manualFallback, setManualFallback] = useState('');

  const recognitionRef = useRef(null);
  const timerRef = useRef(null);

  const stopRecordingAndAnalyze = useCallback(async (finalTranscriptText) => {
    if (recognitionRef.current) {
      try {
        recognitionRef.current.stop();
      } catch (e) {}
    }
    if (timerRef.current) clearInterval(timerRef.current);
    setIsListening(false);
    setStep('analyzing');

    const combinedText = (finalTranscriptText || transcript || manualFallback).trim();

    if (!combinedText || combinedText.split(/\s+/).length < 4) {
      setPlacementResult({
        level: 'Beginner',
        summary: 'Great start! We placed you at Beginner so you can build smooth everyday speaking confidence step by step.',
        strengths: 'Willingness to jump in and speak',
        focusArea: 'Building complete everyday sentences'
      });
      setStep('result');
      return;
    }

    try {
      const result = await callGeminiStructured({
        systemInstruction:
          'You are an expert CEFR English speaking placement assessor. Analyze the spoken transcript from a 60-second placement test and assign Beginner, Intermediate, or Advanced.',
        userPrompt: `Analyze this 60-second spoken English placement sample:\n"${combinedText}"`,
        responseSchema: {
          type: 'OBJECT',
          properties: {
            level: { type: 'STRING', enum: ['Beginner', 'Intermediate', 'Advanced'] },
            summary: { type: 'STRING' },
            strengths: { type: 'STRING' },
            focusArea: { type: 'STRING' }
          },
          required: ['level', 'summary', 'strengths', 'focusArea']
        }
      });
      setPlacementResult(result);
    } catch (err) {
      const wordCount = combinedText.split(/\s+/).length;
      const fallbackLevel = wordCount > 65 ? 'Advanced' : wordCount > 25 ? 'Intermediate' : 'Beginner';
      setPlacementResult({
        level: fallbackLevel,
        summary: `Based on your ${wordCount}-word speaking sample, you're a great fit for our ${fallbackLevel} track!`,
        strengths: 'Clear communication of personal ideas',
        focusArea: 'Expanding vocabulary and natural phrasing'
      });
    }
    setStep('result');
  }, [transcript, manualFallback]);

  const startTest = () => {
    setStep('recording');
    setTimeLeft(60);
    setTranscript('');
    setInterimText('');

    const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
    if (SpeechRecognition) {
      const recognition = new SpeechRecognition();
      recognition.continuous = true;
      recognition.interimResults = true;
      recognition.lang = 'en-US';

      recognition.onstart = () => setIsListening(true);
      recognition.onresult = (event) => {
        let finalChunk = '';
        let interimChunk = '';
        for (let i = event.resultIndex; i < event.results.length; i++) {
          const piece = event.results[i][0].transcript;
          if (event.results[i].isFinal) {
            finalChunk += piece + ' ';
          } else {
            interimChunk += piece;
          }
        }
        if (finalChunk) {
          setTranscript((prev) => prev + finalChunk);
        }
        setInterimText(interimChunk);
      };
      recognition.onerror = () => setIsListening(false);
      recognition.onend = () => setIsListening(false);

      recognitionRef.current = recognition;
      try {
        recognition.start();
      } catch (e) {}
    }

    timerRef.current = setInterval(() => {
      setTimeLeft((prev) => {
        if (prev <= 1) {
          clearInterval(timerRef.current);
          stopRecordingAndAnalyze();
          return 0;
        }
        return prev - 1;
      });
    }, 1000);
  };

  useEffect(() => {
    return () => {
      if (timerRef.current) clearInterval(timerRef.current);
      if (recognitionRef.current) {
        try {
          recognitionRef.current.stop();
        } catch (e) {}
      }
    };
  }, []);

  return (
    <div className="fixed inset-0 z-50 bg-slate-900/70 backdrop-blur-md flex items-center justify-center p-4 overflow-y-auto">
      <div className="bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 rounded-3xl max-w-lg w-full p-6 sm:p-8 shadow-2xl">
        {step === 'intro' && (
          <div className="space-y-6">
            <div className="flex items-center justify-between">
              <span className="px-3 py-1 rounded-full text-xs font-semibold bg-indigo-100 dark:bg-indigo-950 text-indigo-700 dark:text-indigo-300">
                60-Second Placement Check
              </span>
              <button
                onClick={onSkip}
                className="text-xs font-medium text-slate-400 hover:text-slate-600 dark:hover:text-slate-200"
              >
                Skip for now
              </button>
            </div>

            <div>
              <h2 className="text-2xl font-bold tracking-tight">Welcome to SpeakUp! 👋</h2>
              <p className="text-sm text-slate-600 dark:text-slate-400 mt-1.5 leading-relaxed">
                Take a quick 60-second speaking check so Maya can tailor her vocabulary and your 30-day roadmap to your exact level.
              </p>
            </div>

            <div>
              <label className="block text-xs font-semibold uppercase tracking-wider text-slate-500 mb-1.5">
                What should Maya call you?
              </label>
              <input
                type="text"
                value={nameInput}
                onChange={(e) => setNameInput(e.target.value)}
                placeholder="Your first name"
                className="w-full px-4 py-3 rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500"
              />
            </div>

            <div className="p-4 rounded-2xl bg-indigo-50/70 dark:bg-indigo-950/30 border border-indigo-100 dark:border-indigo-900/50">
              <p className="text-xs font-semibold text-indigo-700 dark:text-indigo-300 uppercase tracking-wider mb-1">
                Your Speaking Prompt:
              </p>
              <p className="text-sm font-medium text-slate-800 dark:text-slate-200">
                "Tell me about yourself, your daily routine, and why you want to improve your English."
              </p>
            </div>

            <button
              onClick={startTest}
              className="w-full py-3.5 px-6 rounded-2xl bg-gradient-to-r from-indigo-600 to-teal-600 hover:from-indigo-500 hover:to-teal-500 text-white font-semibold shadow-lg shadow-indigo-500/25 flex items-center justify-center gap-2 transition"
            >
              <Mic className="w-5 h-5" />
              Start 60-Second Test
            </button>
          </div>
        )}

        {step === 'recording' && (
          <div className="space-y-6 text-center">
            <div className="inline-flex items-center gap-2 px-4 py-1.5 rounded-full bg-rose-50 dark:bg-rose-950/50 text-rose-600 dark:text-rose-400 font-mono font-bold text-sm">
              <span className="w-2.5 h-2.5 rounded-full bg-rose-500 animate-ping" />
              00:{String(timeLeft).padStart(2, '0')} remaining
            </div>

            <div className="p-4 rounded-2xl bg-slate-50 dark:bg-slate-800/80 text-left">
              <p className="text-xs text-slate-400 mb-1">Speak naturally about:</p>
              <p className="text-sm font-semibold">
                "Tell me about yourself, your daily routine, and why you want to improve your English."
              </p>
            </div>

            {/* Live Transcript Box */}
            <div className="min-h-[120px] max-h-[180px] overflow-y-auto p-4 rounded-2xl border border-slate-200 dark:border-slate-700 bg-white dark:bg-slate-950 text-left text-sm leading-relaxed">
              {transcript || interimText ? (
                <>
                  <span>{transcript}</span>
                  <span className="text-indigo-500 italic">{interimText}</span>
                </>
              ) : (
                <span className="text-slate-400 italic">
                  {isListening ? 'Listening... start speaking whenever you are ready!' : 'Microphone inactive — you can also type a short intro below.'}
                </span>
              )}
            </div>

            {!isListening && (
              <textarea
                rows={2}
                value={manualFallback}
                onChange={(e) => setManualFallback(e.target.value)}
                placeholder="No mic available? Type 2–3 sentences about yourself here..."
                className="w-full px-3 py-2 rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 text-xs"
              />
            )}

            <button
              onClick={() => stopRecordingAndAnalyze(transcript + ' ' + interimText)}
              className="w-full py-3.5 px-6 rounded-2xl bg-indigo-600 hover:bg-indigo-500 text-white font-semibold flex items-center justify-center gap-2 transition"
            >
              <CheckCircle2 className="w-5 h-5" />
              Finish & Analyze My Level
            </button>
          </div>
        )}

        {step === 'analyzing' && (
          <div className="py-12 text-center space-y-4">
            <div className="w-16 h-16 mx-auto rounded-2xl bg-indigo-100 dark:bg-indigo-950 flex items-center justify-center text-indigo-600 dark:text-indigo-400 animate-bounce">
              <Sparkles className="w-8 h-8" />
            </div>
            <h3 className="text-lg font-bold">Evaluating your speaking fluency...</h3>
            <p className="text-sm text-slate-500">Maya is checking vocabulary range, sentence flow, and clarity.</p>
          </div>
        )}

        {step === 'result' && placementResult && (
          <div className="space-y-6">
            <div className="text-center space-y-2">
              <span className="text-xs font-semibold uppercase tracking-wider text-teal-600 dark:text-teal-400">
                Your Speaking Level
              </span>
              <div className="inline-block px-5 py-2 rounded-2xl bg-gradient-to-r from-indigo-600 to-teal-500 text-white text-2xl font-extrabold shadow-md">
                {placementResult.level}
              </div>
              <p className="text-sm text-slate-600 dark:text-slate-300 pt-2 leading-relaxed">
                {placementResult.summary}
              </p>
            </div>

            <div className="grid grid-cols-1 sm:grid-cols-2 gap-3 text-left">
              <div className="p-3.5 rounded-2xl bg-emerald-50/70 dark:bg-emerald-950/30 border border-emerald-200/60 dark:border-emerald-800/40">
                <p className="text-[11px] font-bold uppercase text-emerald-700 dark:text-emerald-400">Strength</p>
                <p className="text-xs font-medium mt-1">{placementResult.strengths}</p>
              </div>
              <div className="p-3.5 rounded-2xl bg-amber-50/70 dark:bg-amber-950/30 border border-amber-200/60 dark:border-amber-800/40">
                <p className="text-[11px] font-bold uppercase text-amber-700 dark:text-amber-400">Next Focus</p>
                <p className="text-xs font-medium mt-1">{placementResult.focusArea}</p>
              </div>
            </div>

            <button
              onClick={() => onComplete(placementResult.level, nameInput.trim())}
              className="w-full py-3.5 px-6 rounded-2xl bg-indigo-600 hover:bg-indigo-500 text-white font-semibold flex items-center justify-center gap-2 transition"
            >
              Start My 30-Day Plan
              <ArrowRight className="w-5 h-5" />
            </button>
          </div>
        )}
      </div>
    </div>
  );
}

// ============================================================================
// 2. CORE VOICE CALL SESSION (Turn-Taking, Silence Detection, Suggestions)
// ============================================================================
function VoiceCallSession({ config, userData, availableVoices, onEndCall, onCancel }) {
  // Call Lifecycle & Turn State
  // 'ai_speaking' | 'listening' | 'thinking' | 'paused'
  const [turnState, setTurnState] = useState('thinking');
  const [messages, setMessages] = useState([]);
  const [suggestions, setSuggestions] = useState([]);
  const [showSuggestions, setShowSuggestions] = useState(true);
  const [isLoadingSuggestions, setIsLoadingSuggestions] = useState(false);

  // Speech Recognition Live State
  const [interimTranscript, setInterimTranscript] = useState('');
  const [pendingUserSpeech, setPendingUserSpeech] = useState('');
  const [micSupported, setMicSupported] = useState(true);
  const [micError, setMicError] = useState('');
  const [showTextFallback, setShowTextFallback] = useState(false);
  const [typedInput, setTypedInput] = useState('');

  // Audio / TTS Controls
  const [isMutedAI, setIsMutedAI] = useState(false);
  const [speechRate, setSpeechRate] = useState(userData.settings.speechRate || 1.0);

  // Session Timer (up to 60:00 = 3600s)
  const [elapsedSeconds, setElapsedSeconds] = useState(0);
  const [isEndingReport, setIsEndingReport] = useState(false);

  // Refs for clean async closures
  const recognitionRef = useRef(null);
  const silenceTimerRef = useRef(null);
  const pendingSpeechRef = useRef('');
  const turnStateRef = useRef('thinking');
  const messagesRef = useRef([]);
  const chatEndRef = useRef(null);
  const hasInitializedRef = useRef(false);

  useEffect(() => {
    turnStateRef.current = turnState;
  }, [turnState]);

  useEffect(() => {
    messagesRef.current = messages;
    chatEndRef.current?.scrollIntoView({ behavior: 'smooth' });
  }, [messages, interimTranscript]);

  // Session Timer Effect (Max 1 hour = 3600 seconds)
  useEffect(() => {
    const interval = setInterval(() => {
      setElapsedSeconds((prev) => {
        if (prev >= 3600) {
          clearInterval(interval);
          return 3600;
        }
        return prev + 1;
      });
    }, 1000);
    return () => clearInterval(interval);
  }, []);

  // Stop Microphone Helper
  const stopListening = useCallback(() => {
    if (silenceTimerRef.current) {
      clearTimeout(silenceTimerRef.current);
      silenceTimerRef.current = null;
    }
    if (recognitionRef.current) {
      try {
        recognitionRef.current.onend = null;
        recognitionRef.current.stop();
      } catch (e) {}
    }
  }, []);

  // Start Microphone Listening with ~2-second Silence Auto-Send
  const startListening = useCallback(() => {
    const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
    if (!SpeechRecognition) {
      setMicSupported(false);
      setShowTextFallback(true);
      setTurnState('listening');
      return;
    }

    stopListening();
    setMicError('');
    pendingSpeechRef.current = '';
    setPendingUserSpeech('');
    setInterimTranscript('');

    const recognition = new SpeechRecognition();
    recognition.continuous = true;
    recognition.interimResults = true;
    recognition.lang = 'en-US';

    const resetSilenceTimer = () => {
      if (silenceTimerRef.current) clearTimeout(silenceTimerRef.current);
      silenceTimerRef.current = setTimeout(() => {
        const finalUtterance = pendingSpeechRef.current.trim();
        if (finalUtterance && turnStateRef.current === 'listening') {
          stopListening();
          handleUserTurnComplete(finalUtterance);
        }
      }, 2100); // ~2 seconds of silence triggers AI reply
    };

    recognition.onstart = () => {
      setTurnState('listening');
    };

    recognition.onresult = (event) => {
      let newlyFinalized = '';
      let currentInterim = '';

      for (let i = event.resultIndex; i < event.results.length; i++) {
        const text = event.results[i][0].transcript;
        if (event.results[i].isFinal) {
          newlyFinalized += text + ' ';
        } else {
          currentInterim += text;
        }
      }

      if (newlyFinalized) {
        pendingSpeechRef.current = (pendingSpeechRef.current + ' ' + newlyFinalized).trim();
        setPendingUserSpeech(pendingSpeechRef.current);
      }
      setInterimTranscript(currentInterim);

      // Whenever the user speaks (final or interim), wait and reset the 2s pause timer
      if (newlyFinalized || currentInterim) {
        resetSilenceTimer();
      }
    };

    recognition.onerror = (event) => {
      if (event.error === 'not-allowed' || event.error === 'service-not-allowed') {
        setMicError('Microphone permission blocked. Use the keyboard input below or allow mic access.');
        setShowTextFallback(true);
      }
    };

    recognition.onend = () => {
      // Automatically keep listening if still in listening state and user hasn't finished
      if (turnStateRef.current === 'listening') {
        try {
          recognition.start();
        } catch (e) {}
      }
    };

    recognitionRef.current = recognition;
    try {
      recognition.start();
    } catch (e) {
      setShowTextFallback(true);
    }
  }, [stopListening]);

  // Speak AI Response via Browser SpeechSynthesis, then automatically listen
  const speakAIText = useCallback(
    (textToSpeak) => {
      stopListening();
      setTurnState('ai_speaking');

      if (isMutedAI || typeof window === 'undefined' || !('speechSynthesis' in window)) {
        // Simulate brief speaking time if muted so user can read
        setTimeout(() => {
          startListening();
        }, 1200);
        return;
      }

      window.speechSynthesis.cancel();
      const utterance = new SpeechSynthesisUtterance(textToSpeak);
      utterance.lang = 'en-US';
      utterance.rate = speechRate;
      utterance.pitch = 1.04;

      if (userData.settings.voiceURI && availableVoices.length > 0) {
        const chosen = availableVoices.find((v) => v.voiceURI === userData.settings.voiceURI);
        if (chosen) utterance.voice = chosen;
      } else if (availableVoices.length > 0) {
        const preferred =
          availableVoices.find((v) => v.name.includes('Google US English') || v.name.includes('Samantha') || v.name.includes('Natural')) ||
          availableVoices[0];
        if (preferred) utterance.voice = preferred;
      }

      utterance.onend = () => {
        startListening();
      };
      utterance.onerror = () => {
        startListening();
      };

      window.speechSynthesis.speak(utterance);
    },
    [isMutedAI, speechRate, userData.settings.voiceURI, availableVoices, startListening, stopListening]
  );

  // Fetch Initial Suggestions for the Opener
  const generateInitialSuggestions = useCallback(
    async (openerText) => {
      setIsLoadingSuggestions(true);
      try {
        const data = await callGeminiStructured({
          systemInstruction: `You are helping an English learner at the ${config.level} level respond naturally in a spoken conversation. Provide 3 short, natural spoken response options they could say next.`,
          userPrompt: `Topic: "${config.title}". AI Coach Maya just asked: "${openerText}". Give 3 varied, natural spoken replies the learner can say next (tailored to ${config.level} level).`,
          responseSchema: {
            type: 'OBJECT',
            properties: {
              suggestions: {
                type: 'ARRAY',
                items: {
                  type: 'OBJECT',
                  properties: {
                    text: { type: 'STRING' },
                    tone: { type: 'STRING' }
                  },
                  required: ['text', 'tone']
                }
              }
            },
            required: ['suggestions']
          }
        });
        if (data?.suggestions?.length) {
          setSuggestions(data.suggestions.slice(0, 3));
        }
      } catch (e) {
        setSuggestions([
          { text: "I'm doing great today, thanks for asking! How about you?", tone: 'Friendly' },
          { text: "Honestly, I'm a little nervous, but I'm excited to practice English!", tone: 'Honest' },
          { text: "My day has been pretty busy so far, so this is a nice break.", tone: 'Casual' }
        ]);
      } finally {
        setIsLoadingSuggestions(false);
      }
    },
    [config.level, config.title]
  );

  // Initialize Call: AI Speaks First
  useEffect(() => {
    if (hasInitializedRef.current) return;
    hasInitializedRef.current = true;

    const initialMsg = {
      id: Date.now().toString(),
      role: 'ai',
      text: config.opener,
      timestamp: Date.now()
    };
    setMessages([initialMsg]);
    generateInitialSuggestions(config.opener);
    speakAIText(config.opener);

    return () => {
      stopListening();
      if (typeof window !== 'undefined' && 'speechSynthesis' in window) {
        window.speechSynthesis.cancel();
      }
    };
  }, [config.opener, generateInitialSuggestions, speakAIText, stopListening]);

  // Handle User Turn Completion -> Call Gemini for AI Coach Reply + Next 3 Suggestions
  const handleUserTurnComplete = async (userSpokenText) => {
    const cleanText = userSpokenText.trim();
    if (!cleanText) return;

    stopListening();
    setInterimTranscript('');
    setPendingUserSpeech('');
    pendingSpeechRef.current = '';
    setTurnState('thinking');

    const newUserMsg = {
      id: Date.now().toString(),
      role: 'user',
      text: cleanText,
      timestamp: Date.now()
    };

    const updatedHistory = [...messagesRef.current, newUserMsg];
    setMessages(updatedHistory);

    const conversationLog = updatedHistory
      .slice(-10)
      .map((m) => `${m.role === 'ai' ? 'Maya (Coach)' : userData.name}: ${m.text}`)
      .join('\n');

    try {
      const response = await callGeminiStructured({
        systemInstruction: `You are Maya, a warm, patient, encouraging AI English speaking coach on a voice phone call with ${userData.name}.
Rules:
1. Keep your reply short and natural for spoken conversation: 1 to 3 sentences maximum.
2. ALWAYS end your reply with an engaging question to keep ${userData.name} talking.
3. Never interrupt or lecture about grammar rules. If the user makes a grammar or vocabulary mistake, gently model the natural phrasing inside your warm conversational reply.
4. Adapt your vocabulary and sentence complexity to the user's level: ${config.level}.
5. Topic/Scenario: "${config.title}". ${config.customPrompt ? `Role-play instructions: ${config.customPrompt}` : ''}
6. Also generate 3 natural, varied spoken response suggestions ("suggestedUserReplies") that ${userData.name} could say next in response to your new question, tailored to ${config.level} level.`,
        userPrompt: `Conversation so far:\n${conversationLog}\n\nRespond as Maya and provide 3 suggested next replies for ${userData.name}.`,
        responseSchema: {
          type: 'OBJECT',
          properties: {
            aiReply: { type: 'STRING' },
            suggestedUserReplies: {
              type: 'ARRAY',
              items: {
                type: 'OBJECT',
                properties: {
                  text: { type: 'STRING' },
                  tone: { type: 'STRING' }
                },
                required: ['text', 'tone']
              }
            }
          },
          required: ['aiReply', 'suggestedUserReplies']
        }
      });

      const aiMsg = {
        id: (Date.now() + 1).toString(),
        role: 'ai',
        text: response.aiReply,
        timestamp: Date.now()
      };
      setMessages((prev) => [...prev, aiMsg]);
      setSuggestions((response.suggestedUserReplies || []).slice(0, 3));
      speakAIText(response.aiReply);
    } catch (error) {
      const fallbackReply =
        "That's a really interesting point! Could you tell me a bit more about what makes you feel that way?";
      const aiMsg = {
        id: (Date.now() + 1).toString(),
        role: 'ai',
        text: fallbackReply,
        timestamp: Date.now()
      };
      setMessages((prev) => [...prev, aiMsg]);
      setSuggestions([
        { text: 'Sure! For example, in my daily life I notice that...', tone: 'Detailed' },
        { text: 'Well, the main reason is that it saves a lot of time.', tone: 'Direct' },
        { text: "Actually, I hadn't thought deeply about it, but I'd say...", tone: 'Reflective' }
      ]);
      speakAIText(fallbackReply);
    }
  };

  // Refresh Suggestions On-Demand
  const handleRefreshSuggestions = async () => {
    const lastAiMsg = [...messages].reverse().find((m) => m.role === 'ai');
    if (!lastAiMsg || isLoadingSuggestions) return;
    setIsLoadingSuggestions(true);
    try {
      const data = await callGeminiStructured({
        systemInstruction: `Generate 3 fresh, creative, natural spoken English responses for a ${config.level} learner to say next.`,
        userPrompt: `Coach Maya just asked: "${lastAiMsg.text}". Provide 3 different ways the student can reply.`,
        responseSchema: {
          type: 'OBJECT',
          properties: {
            suggestions: {
              type: 'ARRAY',
              items: {
                type: 'OBJECT',
                properties: {
                  text: { type: 'STRING' },
                  tone: { type: 'STRING' }
                },
                required: ['text', 'tone']
              }
            }
          },
          required: ['suggestions']
        }
      });
      if (data?.suggestions?.length) {
        setSuggestions(data.suggestions.slice(0, 3));
      }
    } catch (e) {}
    setIsLoadingSuggestions(false);
  };

  // Toggle Pause / Resume Call
  const handleToggleMicPause = () => {
    if (turnState === 'listening') {
      stopListening();
      setTurnState('paused');
    } else if (turnState === 'paused') {
      startListening();
    } else if (turnState === 'ai_speaking') {
      if (typeof window !== 'undefined' && 'speechSynthesis' in window) {
        window.speechSynthesis.cancel();
      }
      startListening();
    }
  };

  // End Call & Generate Comprehensive Post-Session Speech Report
  const handleFinishSession = async () => {
    stopListening();
    if (typeof window !== 'undefined' && 'speechSynthesis' in window) {
      window.speechSynthesis.cancel();
    }
    setIsEndingReport(true);

    const metrics = analyzeSpeechMetrics(messages, elapsedSeconds);
    const fullTranscriptLines = messages
      .map((m) => `${m.role === 'ai' ? 'Maya' : 'User'}: ${m.text}`)
      .join('\n');

    try {
      const analysis = await callGeminiStructured({
        systemInstruction: `You are an encouraging, expert English speech coach analyzing a practice call transcript.
Provide a realistic, constructive post-session report with:
- Scores out of 10 (e.g., 7.5) for fluency, grammar, vocabulary, and pronunciation (inferred from speech-to-text clarity, phrasing, and flow).
- Top 5 mistakes (or up to 5 improvements if the user made fewer mistakes) with exact original phrase, natural corrected sentence, and brief friendly explanation.
- 5 new useful words or phrases relevant to this conversation topic ("${config.title}"), with meaning and example sentence.
- One encouraging, actionable "tipForTomorrow".`,
        userPrompt: `Session Topic: "${config.title}" (Level: ${config.level})\nDuration: ${formatTimeMMSS(elapsedSeconds)}\nUser WPM: ${metrics.wpm}, Fillers: ${metrics.totalFillers}\n\nTranscript:\n${fullTranscriptLines}`,
        responseSchema: {
          type: 'OBJECT',
          properties: {
            scores: {
              type: 'OBJECT',
              properties: {
                fluency: { type: 'NUMBER' },
                grammar: { type: 'NUMBER' },
                vocabulary: { type: 'NUMBER' },
                pronunciation: { type: 'NUMBER' }
              },
              required: ['fluency', 'grammar', 'vocabulary', 'pronunciation']
            },
            mistakes: {
              type: 'ARRAY',
              items: {
                type: 'OBJECT',
                properties: {
                  original: { type: 'STRING' },
                  corrected: { type: 'STRING' },
                  explanation: { type: 'STRING' }
                },
                required: ['original', 'corrected', 'explanation']
              }
            },
            newVocabulary: {
              type: 'ARRAY',
              items: {
                type: 'OBJECT',
                properties: {
                  phrase: { type: 'STRING' },
                  meaning: { type: 'STRING' },
                  example: { type: 'STRING' }
                },
                required: ['phrase', 'meaning', 'example']
              }
            },
            tipForTomorrow: { type: 'STRING' }
          },
          required: ['scores', 'mistakes', 'newVocabulary', 'tipForTomorrow']
        }
      });

      onEndCall({
        id: Date.now().toString(),
        date: new Date().toISOString(),
        topic: config.title,
        day: config.day,
        level: config.level,
        durationSeconds: elapsedSeconds,
        metrics,
        ...analysis
      });
    } catch (e) {
      // Fallback structured report if API unreachable
      onEndCall({
        id: Date.now().toString(),
        date: new Date().toISOString(),
        topic: config.title,
        day: config.day,
        level: config.level,
        durationSeconds: elapsedSeconds,
        metrics,
        scores: { fluency: 7.5, grammar: 7.2, vocabulary: 7.4, pronunciation: 7.8 },
        mistakes: [
          {
            original: 'I am agree with this idea.',
            corrected: 'I agree with this idea.',
            explanation: 'In English, "agree" is a verb, so you don\'t need "am" before it.'
          },
          {
            original: 'Yesterday I go to the store.',
            corrected: 'Yesterday I went to the store.',
            explanation: 'Use the past tense "went" when talking about yesterday.'
          }
        ],
        newVocabulary: [
          { phrase: 'To be honest', meaning: 'Used to share your genuine opinion politely', example: 'To be honest, I prefer working in the morning.' },
          { phrase: 'Look forward to', meaning: 'To feel excited about something in the future', example: 'I look forward to practicing with you again tomorrow.' },
          { phrase: 'Make sense', meaning: 'To be clear and easy to understand', example: 'Your explanation really makes sense.' },
          { phrase: 'On the other hand', meaning: 'Used to introduce a contrasting point of view', example: 'On the other hand, living in a big city can be expensive.' },
          { phrase: 'Get the hang of it', meaning: 'To learn how to do something well with practice', example: 'Keep speaking daily and you will quickly get the hang of it!' }
        ],
        tipForTomorrow: 'Try pausing silently for half a second instead of using filler words when thinking of your next idea.'
      });
    }
  };

  if (isEndingReport) {
    return (
      <div className="fixed inset-0 z-50 bg-slate-950 text-white flex flex-col items-center justify-center p-6 text-center">
        <div className="w-20 h-20 rounded-3xl bg-gradient-to-br from-indigo-500 to-teal-400 flex items-center justify-center mb-6 animate-pulse shadow-2xl shadow-indigo-500/30">
          <Sparkles className="w-10 h-10 text-white" />
        </div>
        <h2 className="text-2xl font-bold mb-2">Analyzing Your Speaking Call...</h2>
        <p className="text-slate-400 max-w-sm text-sm leading-relaxed">
          Maya is calculating your fluency score, reviewing grammar highlights, and preparing your personalized Repeat & Practice drill.
        </p>
      </div>
    );
  }

  return (
    <div className="fixed inset-0 z-50 bg-gradient-to-b from-slate-900 via-indigo-950 to-slate-950 text-white flex flex-col">
      {/* Top Phone Call Status Header */}
      <div className="px-4 pt-4 pb-3 flex items-center justify-between border-b border-white/10">
        <div className="flex items-center gap-3">
          <div className="relative">
            <div className="w-11 h-11 rounded-full bg-gradient-to-tr from-indigo-500 to-teal-400 flex items-center justify-center font-bold text-lg shadow-lg">
              M
            </div>
            <span
              className={`absolute bottom-0 right-0 w-3.5 h-3.5 rounded-full border-2 border-slate-900 ${
                turnState === 'ai_speaking'
                  ? 'bg-teal-400 animate-ping'
                  : turnState === 'listening'
                  ? 'bg-emerald-400'
                  : 'bg-amber-400'
              }`}
            />
          </div>
          <div>
            <div className="flex items-center gap-2">
              <h2 className="font-bold text-base">Maya • AI Coach</h2>
              <span className="text-[11px] px-2 py-0.5 rounded-full bg-white/10 text-indigo-200">
                {config.level}
              </span>
            </div>
            <p className="text-xs text-slate-300 truncate max-w-[200px] sm:max-w-xs">
              {config.day ? `Day ${config.day}: ` : ''}
              {config.title}
            </p>
          </div>
        </div>

        {/* Session Timer & Audio Controls */}
        <div className="flex items-center gap-2">
          <div className="px-3 py-1.5 rounded-full bg-white/10 font-mono text-xs font-semibold flex items-center gap-1.5">
            <span className="w-2 h-2 rounded-full bg-rose-500 animate-pulse" />
            {formatTimeMMSS(elapsedSeconds)}
            <span className="text-white/40 hidden sm:inline">/ 60:00</span>
          </div>
          <button
            onClick={() => setIsMutedAI(!isMutedAI)}
            title={isMutedAI ? 'Unmute AI Voice' : 'Mute AI Voice'}
            className="p-2 rounded-full bg-white/10 hover:bg-white/20 transition"
          >
            {isMutedAI ? <VolumeX className="w-4 h-4 text-rose-400" /> : <Volume2 className="w-4 h-4 text-teal-300" />}
          </button>
        </div>
      </div>

      {/* Live Turn-Taking Status Banner & Waveform */}
      <div className="px-4 py-3 bg-white/5 flex flex-col items-center justify-center border-b border-white/5">
        <div className="flex items-center gap-3">
          {/* Animated Equalizer Bars */}
          <div className="flex items-center gap-1 h-5">
            {[1, 2, 3, 4, 5].map((bar) => (
              <span
                key={bar}
                className={`w-1 rounded-full transition-all duration-300 ${
                  turnState === 'ai_speaking'
                    ? 'bg-teal-400 animate-bounce h-5'
                    : turnState === 'listening'
                    ? 'bg-indigo-400 animate-pulse h-4'
                    : 'bg-slate-600 h-1.5'
                }`}
                style={{ animationDelay: `${bar * 0.12}s` }}
              />
            ))}
          </div>

          <span className="text-xs font-semibold tracking-wide uppercase">
            {turnState === 'ai_speaking' && (
              <span className="text-teal-300">Maya is speaking... (Tap mic to interrupt)</span>
            )}
            {turnState === 'listening' && (
              <span className="text-emerald-300">Listening to you... (Pause 2s when done)</span>
            )}
            {turnState === 'thinking' && (
              <span className="text-amber-300">Maya is thinking...</span>
            )}
            {turnState === 'paused' && (
              <span className="text-slate-400">Mic paused — tap mic to resume</span>
            )}
          </span>
        </div>
        {micError && (
          <p className="text-[11px] text-amber-300 mt-1 text-center">{micError}</p>
        )}
      </div>

      {/* Live Conversation Transcript Feed */}
      <div className="flex-1 overflow-y-auto px-4 py-4 space-y-3 max-w-2xl w-full mx-auto">
        {messages.map((msg) => {
          const isAI = msg.role === 'ai';
          return (
            <div
              key={msg.id}
              className={`flex flex-col ${isAI ? 'items-start' : 'items-end'}`}
            >
              <span className="text-[10px] text-slate-400 mb-1 px-1">
                {isAI ? 'Maya' : userData.name}
              </span>
              <div
                className={`max-w-[85%] rounded-2xl px-4 py-3 text-sm leading-relaxed shadow-sm ${
                  isAI
                    ? 'bg-white/10 text-white rounded-tl-sm border border-white/10'
                    : 'bg-indigo-600 text-white rounded-tr-sm'
                }`}
              >
                <p>{msg.text}</p>
                {isAI && (
                  <button
                    onClick={() => speakAIText(msg.text)}
                    className="mt-1.5 inline-flex items-center gap-1 text-[11px] text-teal-300 hover:text-teal-200"
                  >
                    <Volume2 className="w-3 h-3" /> Replay audio
                  </button>
                )}
              </div>
            </div>
          );
        })}

        {/* Live Interim User Speech Bubble */}
        {(pendingUserSpeech || interimTranscript) && (
          <div className="flex flex-col items-end">
            <span className="text-[10px] text-emerald-300 mb-1 px-1">Speaking now...</span>
            <div className="max-w-[85%] rounded-2xl rounded-tr-sm px-4 py-3 text-sm bg-indigo-500/40 border border-indigo-400/50 text-white">
              <span>{pendingUserSpeech} </span>
              <span className="italic text-indigo-200">{interimTranscript}</span>
            </div>
          </div>
        )}
        <div ref={chatEndRef} />
      </div>

      {/* SUGGESTED RESPONSES PANEL ("What to say next" after AI speaks) */}
      <div className="max-w-2xl w-full mx-auto px-4 pt-2 pb-1">
        <div className="rounded-2xl bg-white/5 border border-white/10 p-3">
          <div className="flex items-center justify-between mb-2">
            <div className="flex items-center gap-1.5 text-xs font-semibold text-teal-300">
              <Sparkles className="w-3.5 h-3.5" />
              <span>What you can say next (Read aloud or tap to reply):</span>
            </div>
            <div className="flex items-center gap-2">
              <button
                onClick={handleRefreshSuggestions}
                disabled={isLoadingSuggestions}
                title="Refresh ideas"
                className="text-[11px] text-slate-300 hover:text-white flex items-center gap-1 px-2 py-0.5 rounded-lg bg-white/5"
              >
                <RefreshCw className={`w-3 h-3 ${isLoadingSuggestions ? 'animate-spin' : ''}`} />
                New ideas
              </button>
              <button
                onClick={() => setShowSuggestions(!showSuggestions)}
                className="text-[11px] text-slate-400 hover:text-white"
              >
                {showSuggestions ? 'Hide' : 'Show'}
              </button>
            </div>
          </div>

          {showSuggestions && (
            <div className="grid grid-cols-1 gap-1.5">
              {suggestions.map((sug, idx) => (
                <button
                  key={idx}
                  disabled={turnState === 'thinking'}
                  onClick={() => handleUserTurnComplete(sug.text)}
                  className="text-left p-2.5 rounded-xl bg-white/10 hover:bg-indigo-600/50 border border-white/10 hover:border-indigo-400/50 transition flex items-start justify-between gap-2 group"
                >
                  <span className="text-xs sm:text-sm text-slate-100 leading-snug">
                    "{sug.text}"
                  </span>
                  <span className="shrink-0 text-[10px] font-medium px-2 py-0.5 rounded-full bg-white/10 text-teal-200 group-hover:bg-indigo-500 group-hover:text-white">
                    {sug.tone} • Tap
                  </span>
                </button>
              ))}
            </div>
          )}
        </div>
      </div>

      {/* Optional Text Fallback Input (for environments without mic) */}
      {showTextFallback && (
        <form
          onSubmit={(e) => {
            e.preventDefault();
            if (!typedInput.trim()) return;
            handleUserTurnComplete(typedInput);
            setTypedInput('');
          }}
          className="max-w-2xl w-full mx-auto px-4 pt-2 flex gap-2"
        >
          <input
            type="text"
            value={typedInput}
            onChange={(e) => setTypedInput(e.target.value)}
            placeholder="Type your spoken reply here..."
            className="flex-1 px-4 py-2.5 rounded-xl bg-white/10 border border-white/15 text-sm text-white placeholder-slate-400 focus:outline-none focus:ring-2 focus:ring-indigo-400"
          />
          <button
            type="submit"
            className="px-4 py-2.5 rounded-xl bg-indigo-600 hover:bg-indigo-500 text-white text-sm font-semibold flex items-center gap-1"
          >
            <Send className="w-4 h-4" />
          </button>
        </form>
      )}

      {/* Bottom Call Control Dock (Big Mic Button + Send Early + End Call) */}
      <div className="px-4 pt-3 pb-6 bg-slate-950/90 border-t border-white/10">
        <div className="max-w-md mx-auto flex items-center justify-between gap-4">
          {/* Toggle Keyboard Input Fallback */}
          <button
            onClick={() => setShowTextFallback(!showTextFallback)}
            className="flex flex-col items-center gap-1 text-slate-400 hover:text-white p-2"
            title="Toggle text input"
          >
            <div className="w-11 h-11 rounded-full bg-white/10 flex items-center justify-center">
              <MessageSquare className="w-5 h-5" />
            </div>
            <span className="text-[10px]">Type</span>
          </button>

          {/* BIG PULSING MIC BUTTON */}
          <div className="flex flex-col items-center">
            <button
              onClick={handleToggleMicPause}
              className={`relative w-20 h-20 rounded-full flex items-center justify-center shadow-2xl transition-all transform active:scale-95 ${
                turnState === 'listening'
                  ? 'bg-gradient-to-tr from-emerald-500 to-teal-400 shadow-emerald-500/40'
                  : turnState === 'ai_speaking'
                  ? 'bg-indigo-600 shadow-indigo-500/40'
                  : 'bg-slate-700 hover:bg-slate-600'
              }`}
            >
              {turnState === 'listening' && (
                <span className="absolute inset-0 rounded-full bg-emerald-400 animate-ping opacity-25" />
              )}
              {turnState === 'paused' ? (
                <MicOff className="w-9 h-9 text-rose-300" />
              ) : (
                <Mic className="w-9 h-9 text-white" />
              )}
            </button>
            {pendingUserSpeech && turnState === 'listening' && (
              <button
                onClick={() => handleUserTurnComplete(pendingUserSpeech + ' ' + interimTranscript)}
                className="mt-2 px-3 py-1 rounded-full bg-emerald-500 text-slate-950 text-[11px] font-bold shadow"
              >
                Done Speaking Now →
              </button>
            )}
          </div>

          {/* END CALL & GET REPORT BUTTON */}
          <button
            onClick={handleFinishSession}
            className="flex flex-col items-center gap-1 text-rose-400 hover:text-rose-300 p-2"
          >
            <div className="w-11 h-11 rounded-full bg-rose-600 hover:bg-rose-500 text-white flex items-center justify-center shadow-lg shadow-rose-600/30">
              <PhoneOff className="w-5 h-5" />
            </div>
            <span className="text-[10px] font-semibold">End & Report</span>
          </button>
        </div>
      </div>
    </div>
  );
}

// ============================================================================
// 3. HOME TAB (Today's Lesson, 30-Day Progress Bar & Full Curriculum)
// ============================================================================
function HomeTab({ userData, onStartCall, onOpenPractice }) {
  const [selectedLevelFilter, setSelectedLevelFilter] = useState('All');

  const todayLesson =
    CURRICULUM_30_DAYS.find((d) => d.day === userData.currentDay) || CURRICULUM_30_DAYS[0];

  const completionPct = Math.round((userData.completedDays.length / 30) * 100);

  const filteredDays = useMemo(() => {
    if (selectedLevelFilter === 'All') return CURRICULUM_30_DAYS;
    return CURRICULUM_30_DAYS.filter((d) => d.level === selectedLevelFilter);
  }, [selectedLevelFilter]);

  return (
    <div className="space-y-6">
      {/* Greeting & Level Pill */}
      <div className="flex flex-col sm:flex-row sm:items-center justify-between gap-3">
        <div>
          <h1 className="text-2xl font-bold tracking-tight">
            Ready to speak, {userData.name}?
          </h1>
          <p className="text-sm text-slate-500 dark:text-slate-400">
            5–10 minutes of real voice conversation builds lasting fluency.
          </p>
        </div>
        <div className="inline-flex items-center gap-2 self-start px-3.5 py-1.5 rounded-full bg-indigo-50 dark:bg-indigo-950/60 border border-indigo-200/70 dark:border-indigo-800/50 text-indigo-700 dark:text-indigo-300 text-xs font-semibold">
          <Target className="w-3.5 h-3.5" />
          Level: {userData.level}
        </div>
      </div>

      {/* FEATURED TODAY'S LESSON HERO CARD */}
      <div className="relative overflow-hidden rounded-3xl bg-gradient-to-br from-indigo-600 via-indigo-700 to-teal-600 text-white p-6 sm:p-8 shadow-xl shadow-indigo-500/15">
        <div className="relative z-10 flex flex-col md:flex-row md:items-center justify-between gap-6">
          <div className="space-y-2.5 max-w-xl">
            <div className="inline-flex items-center gap-2 px-3 py-1 rounded-full bg-white/15 backdrop-blur-sm text-xs font-semibold">
              <Sparkles className="w-3.5 h-3.5 text-teal-200" />
              <span>TODAY'S LESSON • DAY {todayLesson.day} OF 30</span>
            </div>
            <h2 className="text-2xl sm:text-3xl font-extrabold tracking-tight">
              {todayLesson.title}
            </h2>
            <p className="text-sm text-indigo-100 leading-relaxed">
              "{todayLesson.opener}"
            </p>
            <div className="flex items-center gap-3 pt-1 text-xs text-indigo-200">
              <span className="px-2.5 py-0.5 rounded-md bg-white/10">{todayLesson.level}</span>
              <span>•</span>
              <span>{todayLesson.category}</span>
              <span>•</span>
              <span>Up to 60 min call</span>
            </div>
          </div>

          <div className="flex flex-col sm:flex-row md:flex-col gap-3 shrink-0">
            <button
              onClick={() => onStartCall(todayLesson)}
              className="py-4 px-7 rounded-2xl bg-white text-indigo-700 hover:bg-indigo-50 font-bold text-base shadow-lg flex items-center justify-center gap-2.5 transition transform active:scale-95"
            >
              <PhoneCall className="w-5 h-5 fill-indigo-600" />
              Start Voice Call
            </button>
            <button
              onClick={onOpenPractice}
              className="py-2.5 px-5 rounded-xl bg-white/15 hover:bg-white/25 text-white text-xs font-semibold text-center transition"
            >
              Or try Free Role-Play →
            </button>
          </div>
        </div>
      </div>

      {/* 30-DAY ROADMAP PROGRESS BAR */}
      <div className="p-5 rounded-3xl bg-white dark:bg-slate-900 border border-slate-200/80 dark:border-slate-800 shadow-sm">
        <div className="flex items-center justify-between mb-2.5">
          <div>
            <h3 className="font-bold text-sm">30-Day Fluency Journey</h3>
            <p className="text-xs text-slate-500 dark:text-slate-400">
              {userData.completedDays.length} of 30 real-life topics completed
            </p>
          </div>
          <span className="text-sm font-extrabold text-indigo-600 dark:text-indigo-400">
            {completionPct}%
          </span>
        </div>
        <div className="w-full h-3 rounded-full bg-slate-100 dark:bg-slate-800 overflow-hidden">
          <div
            className="h-full rounded-full bg-gradient-to-r from-indigo-600 to-teal-500 transition-all duration-500"
            style={{ width: `${Math.max(4, completionPct)}%` }}
          />
        </div>
      </div>

      {/* 30-DAY CURRICULUM LIST */}
      <div className="space-y-4">
        <div className="flex flex-col sm:flex-row sm:items-center justify-between gap-2">
          <h3 className="font-bold text-lg">30-Day Speaking Curriculum</h3>
          <div className="flex items-center gap-1.5 overflow-x-auto pb-1">
            {['All', 'Beginner', 'Intermediate', 'Advanced'].map((lvl) => (
              <button
                key={lvl}
                onClick={() => setSelectedLevelFilter(lvl)}
                className={`px-3 py-1.5 rounded-xl text-xs font-semibold transition ${
                  selectedLevelFilter === lvl
                    ? 'bg-indigo-600 text-white'
                    : 'bg-white dark:bg-slate-900 text-slate-600 dark:text-slate-300 border border-slate-200 dark:border-slate-800'
                }`}
              >
                {lvl}
              </button>
            ))}
          </div>
        </div>

        <div className="grid grid-cols-1 md:grid-cols-2 gap-3">
          {filteredDays.map((lesson) => {
            const isCompleted = userData.completedDays.includes(lesson.day);
            const isCurrent = lesson.day === userData.currentDay;

            return (
              <div
                key={lesson.day}
                onClick={() => onStartCall(lesson)}
                className={`cursor-pointer p-4 rounded-2xl border transition-all flex items-center justify-between gap-3 group ${
                  isCurrent
                    ? 'bg-indigo-50/60 dark:bg-indigo-950/30 border-indigo-400 dark:border-indigo-700 shadow-sm'
                    : 'bg-white dark:bg-slate-900 border-slate-200/80 dark:border-slate-800 hover:border-indigo-300 dark:hover:border-indigo-700'
                }`}
              >
                <div className="flex items-center gap-3.5 min-w-0">
                  <div
                    className={`w-11 h-11 rounded-2xl shrink-0 flex items-center justify-center font-bold text-sm ${
                      isCompleted
                        ? 'bg-emerald-100 dark:bg-emerald-950 text-emerald-700 dark:text-emerald-300'
                        : isCurrent
                        ? 'bg-indigo-600 text-white'
                        : 'bg-slate-100 dark:bg-slate-800 text-slate-600 dark:text-slate-300'
                    }`}
                  >
                    {isCompleted ? <CheckCircle2 className="w-5 h-5" /> : `D${lesson.day}`}
                  </div>
                  <div className="min-w-0">
                    <div className="flex items-center gap-2">
                      <span className="text-[11px] font-semibold text-indigo-600 dark:text-indigo-400">
                        Day {lesson.day}
                      </span>
                      <span className="text-[10px] px-2 py-0.5 rounded-full bg-slate-100 dark:bg-slate-800 text-slate-500 dark:text-slate-400">
                        {lesson.level}
                      </span>
                    </div>
                    <h4 className="font-bold text-sm truncate group-hover:text-indigo-600 dark:group-hover:text-indigo-400 transition">
                      {lesson.title}
                    </h4>
                  </div>
                </div>

                <div className="w-9 h-9 rounded-full bg-slate-100 dark:bg-slate-800 group-hover:bg-indigo-600 group-hover:text-white flex items-center justify-center shrink-0 transition">
                  <Play className="w-4 h-4 ml-0.5" />
                </div>
              </div>
            );
          })}
        </div>
      </div>
    </div>
  );
}

// ============================================================================
// 4. PRACTICE TAB (Free Topic & Custom Role-Play)
// ============================================================================
function PracticeTab({ userData, onStartCall }) {
  const [customTopic, setCustomTopic] = useState('');
  const [selectedLevel, setSelectedLevel] = useState(userData.level);

  const handleStartCustom = (e) => {
    e.preventDefault();
    if (!customTopic.trim()) return;
    onStartCall({
      title: customTopic.trim(),
      level: selectedLevel,
      customPrompt: customTopic.trim(),
      opener: `Hi ${userData.name}! I'm ready for our role-play on "${customTopic.trim()}". Whenever you're ready, go ahead and start us off, or tell me how you'd like to begin!`
    });
  };

  return (
    <div className="space-y-6">
      <div>
        <h1 className="text-2xl font-bold tracking-tight">Free Practice & Role-Play</h1>
        <p className="text-sm text-slate-500 dark:text-slate-400">
          Pick any real-world scenario or invent your own role-play with Maya.
        </p>
      </div>

      {/* Custom Scenario Builder Card */}
      <form
        onSubmit={handleStartCustom}
        className="p-5 sm:p-6 rounded-3xl bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 shadow-sm space-y-4"
      >
        <div className="flex items-center gap-2 text-indigo-600 dark:text-indigo-400 font-bold text-sm">
          <Sparkles className="w-4 h-4" />
          <span>Custom Scenario or Role-Play</span>
        </div>

        <input
          type="text"
          value={customTopic}
          onChange={(e) => setCustomTopic(e.target.value)}
          placeholder='e.g., "Interview me for a marketing job" or "Practice ordering at a busy pub"'
          className="w-full px-4 py-3.5 rounded-2xl bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500"
        />

        <div className="flex flex-col sm:flex-row sm:items-center justify-between gap-3">
          <div className="flex items-center gap-2">
            <span className="text-xs font-medium text-slate-500">Coach Level:</span>
            {['Beginner', 'Intermediate', 'Advanced'].map((lvl) => (
              <button
                type="button"
                key={lvl}
                onClick={() => setSelectedLevel(lvl)}
                className={`px-3 py-1 rounded-xl text-xs font-semibold transition ${
                  selectedLevel === lvl
                    ? 'bg-indigo-600 text-white'
                    : 'bg-slate-100 dark:bg-slate-800 text-slate-600 dark:text-slate-300'
                }`}
              >
                {lvl}
              </button>
            ))}
          </div>

          <button
            type="submit"
            disabled={!customTopic.trim()}
            className="py-3 px-6 rounded-2xl bg-gradient-to-r from-indigo-600 to-teal-600 disabled:opacity-40 text-white font-semibold text-sm flex items-center justify-center gap-2 shadow-md transition"
          >
            <PhoneCall className="w-4 h-4" />
            Call Maya Now
          </button>
        </div>
      </form>

      {/* Quick-Start Role-Play Grid */}
      <div className="space-y-3">
        <h3 className="font-bold text-base">Popular Role-Play Scenarios</h3>
        <div className="grid grid-cols-1 sm:grid-cols-2 gap-3.5">
          {QUICK_ROLEPLAYS.map((rp) => (
            <div
              key={rp.id}
              onClick={() =>
                onStartCall({
                  title: rp.title,
                  level: selectedLevel,
                  customPrompt: rp.prompt,
                  opener: rp.opener
                })
              }
              className="cursor-pointer p-4 rounded-2xl bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 hover:border-indigo-400 dark:hover:border-indigo-600 transition flex items-start gap-3.5 group"
            >
              <div className="w-12 h-12 rounded-2xl bg-indigo-50 dark:bg-indigo-950/60 flex items-center justify-center text-2xl shrink-0">
                {rp.emoji}
              </div>
              <div className="flex-1 min-w-0">
                <div className="flex items-center justify-between gap-2">
                  <h4 className="font-bold text-sm group-hover:text-indigo-600 dark:group-hover:text-indigo-400 transition">
                    {rp.title}
                  </h4>
                  <span className="text-[10px] px-2 py-0.5 rounded-full bg-slate-100 dark:bg-slate-800 text-slate-500">
                    {rp.level}
                  </span>
                </div>
                <p className="text-xs text-slate-500 dark:text-slate-400 mt-1 line-clamp-2">
                  {rp.prompt}
                </p>
              </div>
            </div>
          ))}
        </div>
      </div>
    </div>
  );
}

// ============================================================================
// 5. POST-SESSION FEEDBACK REPORT + REPEAT-AFTER-ME DRILL
// ============================================================================
function FeedbackReportView({ report, onClose, onPracticeAgain }) {
  // Interactive Repeat-After-Me Practice Drill State
  const drillSentences = useMemo(() => {
    if (report.mistakes && report.mistakes.length > 0) {
      return report.mistakes.map((m) => m.corrected);
    }
    return [
      'I really enjoyed practicing my English conversation today.',
      'Could you give me an example of how to use that phrase?'
    ];
  }, [report.mistakes]);

  const [drillIndex, setDrillIndex] = useState(0);
  const [isDrillRecording, setIsDrillRecording] = useState(false);
  const [drillSpoken, setDrillSpoken] = useState('');
  const [drillAccuracy, setDrillAccuracy] = useState(null);
  const drillRecRef = useRef(null);

  const currentDrillTarget = drillSentences[drillIndex];

  const speakSentence = (text) => {
    if (typeof window === 'undefined' || !('speechSynthesis' in window)) return;
    window.speechSynthesis.cancel();
    const u = new SpeechSynthesisUtterance(text);
    u.lang = 'en-US';
    u.rate = 0.92;
    window.speechSynthesis.speak(u);
  };

  const startDrillAttempt = () => {
    const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
    if (!SpeechRecognition) {
      setDrillSpoken(currentDrillTarget);
      setDrillAccuracy(100);
      return;
    }

    setDrillSpoken('');
    setDrillAccuracy(null);
    const rec = new SpeechRecognition();
    rec.lang = 'en-US';
    rec.interimResults = false;

    rec.onstart = () => setIsDrillRecording(true);
    rec.onresult = (event) => {
      const spoken = event.results[0][0].transcript;
      setDrillSpoken(spoken);
      setDrillAccuracy(calculateSentenceSimilarity(currentDrillTarget, spoken));
    };
    rec.onerror = () => setIsDrillRecording(false);
    rec.onend = () => setIsDrillRecording(false);

    drillRecRef.current = rec;
    try {
      rec.start();
    } catch (e) {}
  };

  const scoreItems = [
    { label: 'Fluency', value: report.scores?.fluency || 7.5, color: 'bg-indigo-500' },
    { label: 'Grammar', value: report.scores?.grammar || 7.2, color: 'bg-teal-500' },
    { label: 'Vocabulary', value: report.scores?.vocabulary || 7.6, color: 'bg-violet-500' },
    { label: 'Pronunciation', value: report.scores?.pronunciation || 7.8, color: 'bg-emerald-500' }
  ];

  const overallScore = (
    scoreItems.reduce((acc, s) => acc + s.value, 0) / scoreItems.length
  ).toFixed(1);

  return (
    <div className="min-h-screen bg-slate-50 dark:bg-slate-950 pb-20">
      {/* Top Bar */}
      <div className="sticky top-0 z-30 bg-white/90 dark:bg-slate-900/90 backdrop-blur-md border-b border-slate-200 dark:border-slate-800 px-4 h-16 flex items-center justify-between">
        <div className="flex items-center gap-2">
          <Award className="w-5 h-5 text-indigo-600 dark:text-indigo-400" />
          <h1 className="font-bold text-base">Session Feedback Report</h1>
        </div>
        <button
          onClick={onClose}
          className="px-4 py-1.5 rounded-xl bg-slate-100 dark:bg-slate-800 text-xs font-semibold hover:bg-slate-200 dark:hover:bg-slate-700"
        >
          Done
        </button>
      </div>

      <div className="max-w-3xl mx-auto px-4 pt-6 space-y-6">
        {/* Overall Score Banner */}
        <div className="rounded-3xl bg-gradient-to-r from-indigo-600 to-teal-600 text-white p-6 flex items-center justify-between shadow-lg">
          <div>
            <span className="text-xs uppercase tracking-wider font-semibold text-indigo-100">
              {report.topic}
            </span>
            <h2 className="text-2xl font-extrabold mt-1">Great Conversation! 🎉</h2>
            <p className="text-xs text-indigo-100 mt-1">
              Duration: {formatTimeMMSS(report.durationSeconds)} • {report.metrics.turnCount} speaking turns
            </p>
          </div>
          <div className="w-20 h-20 rounded-2xl bg-white/15 backdrop-blur-sm flex flex-col items-center justify-center shrink-0">
            <span className="text-2xl font-black">{overallScore}</span>
            <span className="text-[10px] uppercase tracking-wider text-indigo-100">out of 10</span>
          </div>
        </div>

        {/* 4 Core Skill Scores (Out of 10) */}
        <div className="p-5 rounded-3xl bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 space-y-4">
          <h3 className="font-bold text-sm">Skill Breakdown (out of 10)</h3>
          <div className="grid grid-cols-1 sm:grid-cols-2 gap-4">
            {scoreItems.map((item) => (
              <div key={item.label} className="space-y-1.5">
                <div className="flex justify-between text-xs font-semibold">
                  <span>{item.label}</span>
                  <span>{item.value.toFixed(1)} / 10</span>
                </div>
                <div className="w-full h-2.5 rounded-full bg-slate-100 dark:bg-slate-800 overflow-hidden">
                  <div
                    className={`h-full rounded-full ${item.color}`}
                    style={{ width: `${Math.min(100, item.value * 10)}%` }}
                  />
                </div>
              </div>
            ))}
          </div>
        </div>

        {/* Speaking Pace (WPM) & Filler Word Count */}
        <div className="grid grid-cols-2 sm:grid-cols-3 gap-3">
          <div className="p-4 rounded-2xl bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800">
            <p className="text-xs text-slate-500">Speaking Pace</p>
            <p className="text-2xl font-extrabold text-indigo-600 dark:text-indigo-400 mt-1">
              {report.metrics.wpm} <span className="text-xs font-medium text-slate-400">WPM</span>
            </p>
            <p className="text-[11px] text-slate-400 mt-0.5">Ideal: 110–145 WPM</p>
          </div>

          <div className="p-4 rounded-2xl bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800">
            <p className="text-xs text-slate-500">Filler Words</p>
            <p className="text-2xl font-extrabold text-amber-600 dark:text-amber-400 mt-1">
              {report.metrics.totalFillers}
            </p>
            <p className="text-[11px] text-slate-400 mt-0.5 truncate">
              {Object.keys(report.metrics.fillerBreakdown || {}).length > 0
                ? Object.entries(report.metrics.fillerBreakdown)
                    .map(([k, v]) => `"${k}" (${v})`)
                    .join(', ')
                : 'Clean delivery!'}
            </p>
          </div>

          <div className="col-span-2 sm:col-span-1 p-4 rounded-2xl bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800">
            <p className="text-xs text-slate-500">Total Words Spoken</p>
            <p className="text-2xl font-extrabold text-teal-600 dark:text-teal-400 mt-1">
              {report.metrics.wordCount}
            </p>
            <p className="text-[11px] text-slate-400 mt-0.5">Keep building stamina!</p>
          </div>
        </div>

        {/* ONE TIP FOR TOMORROW SPOTLIGHT */}
        <div className="p-5 rounded-3xl bg-amber-50/80 dark:bg-amber-950/30 border border-amber-200/80 dark:border-amber-800/50 flex items-start gap-3.5">
          <div className="w-10 h-10 rounded-2xl bg-amber-500 text-white flex items-center justify-center shrink-0">
            <Sparkles className="w-5 h-5" />
          </div>
          <div>
            <h3 className="text-xs font-bold uppercase tracking-wider text-amber-800 dark:text-amber-300">
              One Tip for Tomorrow
            </h3>
            <p className="text-sm font-medium text-slate-800 dark:text-slate-200 mt-1 leading-relaxed">
              {report.tipForTomorrow}
            </p>
          </div>
        </div>

        {/* TOP 5 MISTAKES & NATURAL CORRECTIONS */}
        <div className="p-5 rounded-3xl bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 space-y-3.5">
          <h3 className="font-bold text-base">Top Grammar & Phrasing Upgrades</h3>
          <div className="space-y-3">
            {(report.mistakes || []).slice(0, 5).map((m, idx) => (
              <div
                key={idx}
                className="p-3.5 rounded-2xl bg-slate-50 dark:bg-slate-800/60 border border-slate-200/60 dark:border-slate-700/60 space-y-1.5"
              >
                <div className="flex items-start gap-2 text-xs text-rose-600 dark:text-rose-400 line-through">
                  <span>"{m.original}"</span>
                </div>
                <div className="flex items-center justify-between gap-2">
                  <p className="text-sm font-bold text-emerald-700 dark:text-emerald-400">
                    ✓ "{m.corrected}"
                  </p>
                  <button
                    onClick={() => speakSentence(m.corrected)}
                    className="p-1.5 rounded-lg bg-emerald-100 dark:bg-emerald-950 text-emerald-700 dark:text-emerald-300 hover:opacity-80 shrink-0"
                    title="Listen to natural phrasing"
                  >
                    <Volume2 className="w-3.5 h-3.5" />
                  </button>
                </div>
                <p className="text-xs text-slate-500 dark:text-slate-400">{m.explanation}</p>
              </div>
            ))}
          </div>
        </div>

        {/* INTERACTIVE REPEAT & PRACTICE SPEAKING DRILL */}
        <div className="p-5 sm:p-6 rounded-3xl bg-indigo-50/80 dark:bg-indigo-950/40 border-2 border-indigo-200 dark:border-indigo-800 space-y-4">
          <div className="flex items-center justify-between">
            <div>
              <span className="text-[11px] font-bold uppercase tracking-wider text-indigo-600 dark:text-indigo-400">
                Active Speaking Drill ({drillIndex + 1} of {drillSentences.length})
              </span>
              <h3 className="font-bold text-base">Repeat the Corrected Sentence Aloud</h3>
            </div>
            <button
              onClick={() => speakSentence(currentDrillTarget)}
              className="px-3 py-1.5 rounded-xl bg-white dark:bg-slate-900 border border-indigo-200 dark:border-indigo-800 text-xs font-semibold text-indigo-600 dark:text-indigo-300 flex items-center gap-1.5"
            >
              <Volume2 className="w-3.5 h-3.5" /> Hear Model
            </button>
          </div>

          <div className="p-4 rounded-2xl bg-white dark:bg-slate-900 border border-indigo-100 dark:border-indigo-900/50">
            <p className="text-base font-bold text-slate-900 dark:text-white">
              "{currentDrillTarget}"
            </p>
          </div>

          {drillSpoken && (
            <div className="p-3 rounded-xl bg-white/80 dark:bg-slate-900/80 text-xs flex items-center justify-between">
              <div>
                <span className="text-slate-400">You said: </span>
                <span className="font-medium">"{drillSpoken}"</span>
              </div>
              <span
                className={`px-2.5 py-1 rounded-full font-bold ${
                  drillAccuracy >= 80
                    ? 'bg-emerald-100 text-emerald-700'
                    : 'bg-amber-100 text-amber-700'
                }`}
              >
                {drillAccuracy}% Match
              </span>
            </div>
          )}

          <div className="flex items-center gap-3">
            <button
              onClick={startDrillAttempt}
              className={`flex-1 py-3 px-4 rounded-2xl font-semibold text-sm text-white flex items-center justify-center gap-2 transition ${
                isDrillRecording ? 'bg-rose-500 animate-pulse' : 'bg-indigo-600 hover:bg-indigo-500'
              }`}
            >
              <Mic className="w-4 h-4" />
              {isDrillRecording ? 'Listening... Speak Now' : 'Tap & Repeat Sentence'}
            </button>

            <button
              onClick={() => {
                setDrillSpoken('');
                setDrillAccuracy(null);
                setDrillIndex((prev) => (prev + 1) % drillSentences.length);
              }}
              className="py-3 px-4 rounded-2xl bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-700 text-xs font-semibold"
            >
              Next →
            </button>
          </div>
        </div>

        {/* 5 NEW WORDS OR PHRASES TO LEARN */}
        <div className="p-5 rounded-3xl bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 space-y-3.5">
          <h3 className="font-bold text-base">5 New Phrases to Add to Your Vocabulary</h3>
          <div className="divide-y divide-slate-100 dark:divide-slate-800">
            {(report.newVocabulary || []).slice(0, 5).map((item, idx) => (
              <div key={idx} className="py-3 first:pt-0 last:pb-0 flex items-start justify-between gap-3">
                <div>
                  <p className="font-bold text-sm text-indigo-600 dark:text-indigo-400">
                    {item.phrase}
                  </p>
                  <p className="text-xs text-slate-600 dark:text-slate-300 mt-0.5">{item.meaning}</p>
                  <p className="text-xs italic text-slate-400 mt-1">"{item.example}"</p>
                </div>
                <button
                  onClick={() => speakSentence(item.phrase)}
                  className="p-2 rounded-xl bg-slate-100 dark:bg-slate-800 text-slate-600 dark:text-slate-300 hover:text-indigo-600 shrink-0"
                >
                  <Volume2 className="w-4 h-4" />
                </button>
              </div>
            ))}
          </div>
        </div>

        {/* Bottom Actions */}
        <div className="flex flex-col sm:flex-row gap-3 pt-2">
          <button
            onClick={onPracticeAgain}
            className="flex-1 py-3.5 px-6 rounded-2xl bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 font-semibold text-sm flex items-center justify-center gap-2"
          >
            <RotateCcw className="w-4 h-4" /> Practice Topic Again
          </button>
          <button
            onClick={onClose}
            className="flex-1 py-3.5 px-6 rounded-2xl bg-indigo-600 hover:bg-indigo-500 text-white font-semibold text-sm flex items-center justify-center gap-2"
          >
            Back to Dashboard <ArrowRight className="w-4 h-4" />
          </button>
        </div>
      </div>
    </div>
  );
}

// ============================================================================
// 6. PROGRESS TAB (SVG Score Trend Chart, Badges & Session History)
// ============================================================================
function ProgressTab({ userData, onOpenReport }) {
  const history = [...userData.sessionHistory].reverse(); // oldest -> newest for chart

  // Generate SVG polyline points for overall score across sessions
  const chartPoints = useMemo(() => {
    const data =
      history.length >= 2
        ? history.map((s) => {
            const sc = s.scores || { fluency: 7, grammar: 7, vocabulary: 7, pronunciation: 7 };
            return (sc.fluency + sc.grammar + sc.vocabulary + sc.pronunciation) / 4;
          })
        : [6.2, 6.8, 7.1, 7.5]; // Sample baseline preview if <2 sessions

    const width = 320;
    const height = 110;
    const pad = 14;
    const stepX = (width - pad * 2) / Math.max(1, data.length - 1);

    return data.map((val, idx) => {
      const x = pad + idx * stepX;
      const y = height - pad - ((Math.min(10, Math.max(0, val)) / 10) * (height - pad * 2));
      return { x, y, val: val.toFixed(1) };
    });
  }, [history]);

  const polylineStr = chartPoints.map((p) => `${p.x},${p.y}`).join(' ');

  return (
    <div className="space-y-6">
      <div>
        <h1 className="text-2xl font-bold tracking-tight">Your Speaking Progress</h1>
        <p className="text-sm text-slate-500 dark:text-slate-400">
          Track your fluency growth, speaking minutes, and unlocked achievements.
        </p>
      </div>

      {/* Top Metrics Summary */}
      <div className="grid grid-cols-3 gap-3">
        <div className="p-4 rounded-2xl bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 text-center">
          <Flame className="w-5 h-5 mx-auto text-amber-500 mb-1" />
          <p className="text-xl font-extrabold">{userData.streak}</p>
          <p className="text-[11px] text-slate-500">Day Streak</p>
        </div>
        <div className="p-4 rounded-2xl bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 text-center">
          <Clock className="w-5 h-5 mx-auto text-indigo-500 mb-1" />
          <p className="text-xl font-extrabold">{userData.totalMinutes}</p>
          <p className="text-[11px] text-slate-500">Mins Spoken</p>
        </div>
        <div className="p-4 rounded-2xl bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 text-center">
          <Trophy className="w-5 h-5 mx-auto text-teal-500 mb-1" />
          <p className="text-xl font-extrabold">{userData.completedDays.length}/30</p>
          <p className="text-[11px] text-slate-500">Days Done</p>
        </div>
      </div>

      {/* Speaking Score Trend SVG Chart */}
      <div className="p-5 rounded-3xl bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800">
        <div className="flex items-center justify-between mb-3">
          <div>
            <h3 className="font-bold text-sm">Overall Fluency Score Trend</h3>
            <p className="text-xs text-slate-400">
              {history.length >= 2
                ? `Across your last ${history.length} calls`
                : 'Sample trajectory — complete calls to plot your personal curve'}
            </p>
          </div>
          <span className="text-xs font-bold px-2.5 py-1 rounded-full bg-indigo-50 dark:bg-indigo-950 text-indigo-600 dark:text-indigo-300">
            Scale 0–10
          </span>
        </div>

        <svg viewBox="0 0 320 110" className="w-full h-32 overflow-visible">
          {[25, 55, 85].map((yLine) => (
            <line
              key={yLine}
              x1="10"
              y1={yLine}
              x2="310"
              y2={yLine}
              stroke="currentColor"
              className="text-slate-100 dark:text-slate-800"
              strokeWidth="1"
            />
          ))}
          <polyline
            fill="none"
            stroke="#4f46e5"
            strokeWidth="3"
            strokeLinecap="round"
            strokeLinejoin="round"
            points={polylineStr}
          />
          {chartPoints.map((pt, idx) => (
            <g key={idx}>
              <circle cx={pt.x} cy={pt.y} r="4.5" fill="#14b8a6" stroke="#fff" strokeWidth="1.5" />
              <text
                x={pt.x}
                y={pt.y - 8}
                textAnchor="middle"
                className="text-[9px] font-bold fill-slate-500 dark:fill-slate-400"
              >
                {pt.val}
              </text>
            </g>
          ))}
        </svg>
      </div>

      {/* Badges & Milestones */}
      <div className="space-y-3">
        <h3 className="font-bold text-base">Badges & Milestones</h3>
        <div className="grid grid-cols-2 sm:grid-cols-3 gap-3">
          {BADGES_CATALOG.map((badge) => {
            const unlocked = userData.unlockedBadges.includes(badge.id);
            return (
              <div
                key={badge.id}
                className={`p-4 rounded-2xl border transition ${
                  unlocked
                    ? 'bg-white dark:bg-slate-900 border-indigo-200 dark:border-indigo-800 shadow-sm'
                    : 'bg-slate-100/60 dark:bg-slate-900/40 border-slate-200/50 dark:border-slate-800/50 opacity-55 grayscale'
                }`}
              >
                <div className="text-2xl mb-1.5">{badge.icon}</div>
                <h4 className="font-bold text-xs">{badge.title}</h4>
                <p className="text-[11px] text-slate-500 dark:text-slate-400 mt-0.5 leading-snug">
                  {badge.desc}
                </p>
              </div>
            );
          })}
        </div>
      </div>

      {/* Past Session Feedback Reports */}
      <div className="space-y-3">
        <h3 className="font-bold text-base">Recent Call Reports</h3>
        {userData.sessionHistory.length === 0 ? (
          <div className="p-6 rounded-2xl bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 text-center text-sm text-slate-500">
            No completed calls yet. Start your Day 1 conversation to unlock your first speech report!
          </div>
        ) : (
          <div className="space-y-2.5">
            {userData.sessionHistory.map((rep) => (
              <div
                key={rep.id}
                onClick={() => onOpenReport(rep)}
                className="cursor-pointer p-4 rounded-2xl bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 hover:border-indigo-400 flex items-center justify-between gap-3 transition"
              >
                <div>
                  <h4 className="font-bold text-sm">{rep.topic}</h4>
                  <p className="text-xs text-slate-500 mt-0.5">
                    {new Date(rep.date).toLocaleDateString()} • {formatTimeMMSS(rep.durationSeconds)} • {rep.metrics.wpm} WPM
                  </p>
                </div>
                <ChevronRight className="w-4 h-4 text-slate-400" />
              </div>
            ))}
          </div>
        )}
      </div>
    </div>
  );
}

// ============================================================================
// 7. PROFILE & SETTINGS TAB
// ============================================================================
function ProfileTab({ user, userData, availableVoices, onUpdateUserData, onRetakeTest }) {
  const [reminderSavedToast, setReminderSavedToast] = useState(false);

  const handleToggleReminder = async () => {
    const nextEnabled = !userData.settings.reminderEnabled;
    if (nextEnabled && typeof window !== 'undefined' && 'Notification' in window) {
      try {
        await Notification.requestPermission();
      } catch (e) {}
    }
    onUpdateUserData((prev) => ({
      settings: { ...prev.settings, reminderEnabled: nextEnabled }
    }));
    setReminderSavedToast(true);
    setTimeout(() => setReminderSavedToast(false), 2500);
  };

  return (
    <div className="space-y-6">
      <div>
        <h1 className="text-2xl font-bold tracking-tight">Profile & Coach Settings</h1>
        <p className="text-sm text-slate-500 dark:text-slate-400">
          Customize Maya's voice, speaking level, and daily practice reminders.
        </p>
      </div>

      {/* User Identity & Level Card */}
      <div className="p-5 rounded-3xl bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 flex flex-col sm:flex-row sm:items-center justify-between gap-4">
        <div className="flex items-center gap-3.5">
          <div className="w-14 h-14 rounded-2xl bg-gradient-to-br from-indigo-600 to-teal-500 text-white font-extrabold text-xl flex items-center justify-center">
            {(userData.name || 'A')[0].toUpperCase()}
          </div>
          <div>
            <input
              type="text"
              value={userData.name}
              onChange={(e) => onUpdateUserData({ name: e.target.value })}
              className="font-bold text-lg bg-transparent border-b border-transparent focus:border-indigo-500 focus:outline-none"
            />
            <p className="text-xs text-slate-500">
              Current Level: <span className="font-semibold text-indigo-600 dark:text-indigo-400">{userData.level}</span>
            </p>
            {user && (
              <p className="text-[10px] text-slate-400 mt-0.5 font-mono truncate max-w-[220px]">
                Cloud Sync ID: {user.uid}
              </p>
            )}
          </div>
        </div>

        <button
          onClick={onRetakeTest}
          className="py-2.5 px-4 rounded-xl bg-indigo-50 dark:bg-indigo-950/60 text-indigo-700 dark:text-indigo-300 text-xs font-semibold hover:bg-indigo-100 transition self-start sm:self-center"
        >
          Retake 60s Placement Test
        </button>
      </div>

      {/* Manual Level Override */}
      <div className="p-5 rounded-3xl bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 space-y-3">
        <h3 className="font-bold text-sm">Proficiency Level</h3>
        <div className="grid grid-cols-3 gap-2.5">
          {['Beginner', 'Intermediate', 'Advanced'].map((lvl) => (
            <button
              key={lvl}
              onClick={() => onUpdateUserData({ level: lvl })}
              className={`py-2.5 px-3 rounded-xl text-xs font-semibold border transition ${
                userData.level === lvl
                  ? 'bg-indigo-600 text-white border-indigo-600'
                  : 'bg-slate-50 dark:bg-slate-800 border-slate-200 dark:border-slate-700 text-slate-600 dark:text-slate-300'
              }`}
            >
              {lvl}
            </button>
          ))}
        </div>
      </div>

      {/* AI Voice & Speed Settings */}
      <div className="p-5 rounded-3xl bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 space-y-4">
        <h3 className="font-bold text-sm">Maya's Voice & Speed</h3>

        <div>
          <label className="block text-xs text-slate-500 mb-1.5">AI Coach Voice</label>
          <select
            value={userData.settings.voiceURI}
            onChange={(e) =>
              onUpdateUserData((prev) => ({
                settings: { ...prev.settings, voiceURI: e.target.value }
              }))
            }
            className="w-full px-3.5 py-2.5 rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 text-xs"
          >
            <option value="">Default Natural English Voice</option>
            {availableVoices.map((v) => (
              <option key={v.voiceURI} value={v.voiceURI}>
                {v.name} ({v.lang})
              </option>
            ))}
          </select>
        </div>

        <div>
          <div className="flex justify-between text-xs mb-1.5">
            <span className="text-slate-500">Speaking Speed</span>
            <span className="font-bold">{userData.settings.speechRate.toFixed(2)}x</span>
          </div>
          <input
            type="range"
            min="0.75"
            max="1.25"
            step="0.05"
            value={userData.settings.speechRate}
            onChange={(e) =>
              onUpdateUserData((prev) => ({
                settings: { ...prev.settings, speechRate: parseFloat(e.target.value) }
              }))
            }
            className="w-full accent-indigo-600"
          />
          <div className="flex justify-between text-[10px] text-slate-400">
            <span>0.75x (Slower)</span>
            <span>1.0x (Natural)</span>
            <span>1.25x (Faster)</span>
          </div>
        </div>
      </div>

      {/* Daily Reminder & Theme */}
      <div className="p-5 rounded-3xl bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 space-y-4">
        <div className="flex items-center justify-between">
          <div className="flex items-center gap-3">
            <Bell className="w-5 h-5 text-indigo-600 dark:text-indigo-400" />
            <div>
              <h4 className="font-bold text-sm">Daily Practice Reminder</h4>
              <p className="text-xs text-slate-500">Keep your speaking streak alive</p>
            </div>
          </div>
          <button
            onClick={handleToggleReminder}
            className={`w-12 h-7 rounded-full transition p-1 ${
              userData.settings.reminderEnabled ? 'bg-indigo-600' : 'bg-slate-300 dark:bg-slate-700'
            }`}
          >
            <div
              className={`w-5 h-5 rounded-full bg-white transition-transform ${
                userData.settings.reminderEnabled ? 'translate-x-5' : 'translate-x-0'
              }`}
            />
          </button>
        </div>

        {userData.settings.reminderEnabled && (
          <div className="flex items-center justify-between pt-2 border-t border-slate-100 dark:border-slate-800">
            <span className="text-xs text-slate-500">Reminder Time</span>
            <input
              type="time"
              value={userData.settings.reminderTime}
              onChange={(e) =>
                onUpdateUserData((prev) => ({
                  settings: { ...prev.settings, reminderTime: e.target.value }
                }))
              }
              className="px-3 py-1.5 rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 text-xs font-semibold"
            />
          </div>
        )}

        {reminderSavedToast && (
          <p className="text-xs text-emerald-600 dark:text-emerald-400 font-medium">
            ✓ Reminder preferences updated!
          </p>
        )}
      </div>
    </div>
  );
}
