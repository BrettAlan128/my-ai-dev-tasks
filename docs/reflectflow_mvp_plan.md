# **ReflectFlow MVP Planning Document**

---

## **1. MVP Scope Definition**

### **Core MVP Features (Required for Launch):**
- ✅ **Freeform Journaling** - Basic text area for user reflections
- ✅ **Prompt Library** - Categorized journaling prompts (text only)
- ✅ **AI Rewrite Option** - Grammar/clarity help using OpenAI API
- ✅ **Feelings/Needs Expander** - Emotional vocabulary guidance
- ✅ **Meditation Generator** - Text-to-speech guided audio meditation
- ✅ **Intention Practice Suggestion** - One-line actionable suggestions
- ✅ **Save & Export** - Save journal entries and meditation links
- ✅ **Daily Prompt Notification** - Optional push/alert with new prompt

### **Explicitly Excluded from MVP:**
- ❌ User history/timeline views
- ❌ Mood tracker charts
- ❌ Emotional mind maps
- ❌ Sharing features
- ❌ Advanced audio customization
- ❌ Multimodal inputs (voice/photo)

---

## **2. Target User Personas (MVP Focus)**

**Primary Users:**
1. **Reflective Explorer** (25-45) - Wants guidance for self-reflection
2. **Burnout Professional** (30-55) - Needs emotional release and restoration  
3. **Therapy Aligned User** (20-50) - Seeks deeper self-awareness between sessions
4. **Emerging Seeker** (20-28) - Recent graduate/early-career seeking direction and purpose

### **Emerging Seeker Details:**
- **Age:** 20-28
- **Stage:** Recent graduate or early-career limbo
- **Motivation:** Seeking direction, clarity, and deeper sense of purpose
- **Needs:** Space to process emotions, explore possibilities, identify personal sources of meaning
- **Frustrations:** Pressure to have answers, disconnection from self, fear of choosing wrong path
- **Why ReflectFlow Helps:** Guided journaling surfaces internal patterns; feeling/need tags create language for direction; meditation and intention practices ground and reinforce clarity over time

---

## **3. Core User Flows (MVP)**

**UC1: Daily Journaling with AI Insight**
- User writes entry (freeform or with prompt)
- AI analyzes entry for tone, feelings, and needs
- AI offers rewrite/suggestions based on emotional analysis
- Entry is saved with emotional analysis data
- System identifies potential emotional anchors

**UC2: Meditation Generation**
- AI analyzes journal tone, feelings, and needs
- System matches against user's emotional anchors and patterns
- AI generates guided meditation script tailored to emotional state
- TTS converts to audio with appropriate pacing/tone
- User listens to personalized meditation

**UC3: Prompt Navigation**
- User selects from prompt library
- System suggests prompts based on emotional patterns and anchors
- User begins writing with guided emotional exploration

**UC4: Intention Practice Suggestion**
- After journaling, AI analyzes emotional state
- System matches against user's needs and anchor patterns
- User receives micro-practice suggestion (breathing, mantra, movement)
- Suggestion is tailored to current emotional state and historical patterns

---

## **4. Data Models (MVP Simplified)**

**User Table:**
- `user_id` (primary)
- `email`, `password_hash`, `created_at`

**Journal Entry Table:**
- `entry_id` (primary)
- `user_id` (foreign key)
- `text`, `timestamp`, `prompt_used`
- `ai_analysis` (JSON field containing tone, feelings, needs analysis)

**Emotional Analysis Table:**
- `analysis_id` (primary)
- `entry_id` (foreign key)
- `primary_tone` (e.g., "anxious", "grateful", "overwhelmed")
- `secondary_tone` (optional)
- `detected_feelings` (JSON array of feelings)
- `identified_needs` (JSON array of needs)
- `emotional_intensity` (1-10 scale)
- `confidence_score` (AI confidence in analysis)

**Emotional Anchors Table:**
- `anchor_id` (primary)
- `user_id` (foreign key)
- `anchor_label` (e.g., "after conflict", "at peace", "aligned with purpose")
- `associated_feelings` (JSON array)
- `associated_needs` (JSON array)
- `created_from_entry_id` (foreign key to journal entry)
- `created_at`, `last_used_at`
- `usage_count` (how often this anchor is referenced)

**Meditation Table:**
- `meditation_id` (primary)
- `entry_id` (foreign key)
- `audio_url`, `script_text`
- `target_tone` (what emotional state this meditation addresses)
- `target_feelings` (JSON array of feelings this meditation helps with)
- `target_needs` (JSON array of needs this meditation supports)
- `anchor_references` (JSON array of anchor IDs that influenced this meditation)

**Intention Practice Table:**
- `practice_id` (primary)
- `entry_id` (foreign key)
- `practice_text`, `practice_type` (e.g., "breathing", "mantra", "movement")
- `target_tone`, `target_feelings`, `target_needs`
- `anchor_references` (JSON array of anchor IDs)

**Prompt Table:**
- `prompt_id` (primary)
- `text`, `category`
- `target_emotional_states` (JSON array of tones this prompt helps explore)
- `target_feelings` (JSON array of feelings this prompt surfaces)
- `target_needs` (JSON array of needs this prompt helps identify)

**AI Recommendation Log Table:**
- `recommendation_id` (primary)
- `user_id` (foreign key)
- `entry_id` (foreign key)
- `recommendation_type` ("meditation", "intention_practice", "prompt")
- `recommended_item_id` (foreign key to respective table)
- `matching_anchors` (JSON array of anchor IDs used)
- `matching_feelings` (JSON array of feelings that influenced recommendation)
- `matching_needs` (JSON array of needs that influenced recommendation)
- `user_feedback` (optional: "helpful", "not_helpful", "neutral")
- `created_at`

---

## **5. AI Matching & Recommendation System**

### **Core Matching Logic:**
The AI system uses a multi-layered approach to match journal entries with appropriate meditations and intention practices:

**Primary Matching Factors:**
1. **Emotional Tone** - Primary emotional state (anxious, grateful, overwhelmed, etc.)
2. **Detected Feelings** - Specific emotions identified in the entry
3. **Identified Needs** - Underlying needs the user is expressing
4. **Emotional Anchors** - Historical patterns and situational contexts

**Matching Algorithm:**
- **Exact Match:** Direct alignment of tone + feelings + needs
- **Partial Match:** 2 out of 3 factors align
- **Anchor-Based Match:** Historical anchor patterns suggest relevant content
- **Fallback Match:** Template-based recommendations when analysis is unclear

### **Recommendation Priority:**
1. **High Priority:** Exact matches with high confidence scores
2. **Medium Priority:** Partial matches with anchor reinforcement
3. **Low Priority:** Template fallbacks for unclear emotional states

### **Learning & Improvement:**
- Track user feedback on recommendations
- Adjust matching weights based on helpfulness ratings
- Build user-specific emotional patterns over time
- Refine anchor associations based on usage patterns

---

## **6. Technical Architecture (Beginner-Friendly)**

### **Recommended Stack:**
- **Backend:** Python + FastAPI (builds on your Python experience)
- **Frontend:** HTML/CSS/JavaScript (start simple, add React later)
- **Database:** SQLite (local development) → PostgreSQL (production)
- **AI Services:** OpenAI API (you have premium subscription)
- **TTS:** ElevenLabs or PlayHT (simple API integration)
- **Hosting:** Vercel (frontend) + Railway (backend)
- **Authentication:** Simple email/password (Firebase Auth or similar)

### **Why This Stack:**
- Leverages your Python experience
- FastAPI is simpler than Django for beginners
- SQLite is perfect for learning database concepts
- You already have OpenAI API access
- Vercel/Railway are beginner-friendly hosting platforms

---

## **7. UI/UX Guidelines (MVP)**

- **Tone:** Calm, minimal, non-distracting
- **Color palette:** Earth tones or grayscale
- **Layout:** Single-page application with clear navigation
- **Input:** Large text area with generous whitespace
- **Flow:** Write → AI assist → meditation → intention practice
- **Mobile-responsive:** Works on phone and desktop

---

## **8. Success Criteria & Metrics**

### **MVP Success Metrics:**
- User can complete full journaling → meditation flow
- AI provides helpful text analysis and suggestions
- Meditation audio generates successfully
- App is stable and responsive
- Basic user authentication works

### **Validation Goals:**
- Test with 5-10 users from target personas
- Gather feedback on AI suggestions quality
- Measure meditation completion rates
- Assess user engagement with prompts

---

## **9. Risk Mitigation**

### **Technical Risks:**
- **AI API failures** → Fallback to template responses
- **TTS generation issues** → Text-only meditation option
- **Database complexity** → Start with simple file storage, migrate later

### **User Experience Risks:**
- **Overwhelming interface** → Start with minimal features
- **Poor AI suggestions** → Allow users to skip/ignore
- **Audio quality issues** → Provide text script as backup

---

## **10. Development Phases (High-Level)**

### **Phase 1: Foundation (Weeks 1-4)**
- Set up development environment
- Build basic FastAPI backend
- Create simple HTML frontend
- Implement user authentication

### **Phase 2: Core Features (Weeks 5-8)**
- Build journaling interface
- Integrate OpenAI API for text analysis
- Create prompt library
- Add basic database storage

### **Phase 3: AI & Audio (Weeks 9-12)**
- Implement meditation generation
- Integrate TTS service
- Add intention practice suggestions
- Polish user experience

### **Phase 4: Polish & Deploy (Weeks 13-16)**
- Error handling and edge cases
- Mobile responsiveness
- Deploy to production
- User testing and feedback

---

## **11. Edge Cases & Error Handling**

- AI misinterprets tone → user can correct or deselect emotional tags
- Meditation fails to generate → fallback to relevant template or retry
- Entry too short or vague → AI gently prompts for elaboration or suggests a prompt
- Multiple tones detected → user selects primary or confirms balance
- Broken audio links → regeneration or alternative playback route

---

## **12. Security & Compliance (MVP Level)**

- **Data Security:** Encrypted storage for user data
- **Authentication:** Secure user login/logout
- **Privacy:** User data export and deletion capabilities
- **Compliance:** Basic GDPR considerations (data export/delete)

---

## **13. Next Steps**

This MVP plan is now ready for:
1. **Epic Creation** - Breaking down into major development chunks
2. **Task Breakdown** - Detailed implementation tasks
3. **Timeline Estimation** - More precise time estimates
4. **Resource Planning** - Tools, services, and learning resources needed

**Ready to proceed with epic/task breakdown?** This plan gives you a solid foundation that builds on your Python experience while introducing web development concepts gradually.

---

*Document created: [Current Date]*
*Based on: reflect_flow_prd.md and mvp_scope_staging.md*
*Target Timeline: 3-4 months with 2 hours/day development* 