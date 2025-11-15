# 👻 Echoes of the Dead Web

**Kiroween Hackathon 2025 - Resurrection Category**

> "Because the internet never forgets… it just needs a séance."

Bring dead websites and lost internet knowledge back to life using Kiro's agentic AI, MCP extensions, and spec-driven development. A browser plugin + web app that lets users "converse with dead websites" like GeoCities, MySpace 2005, or defunct forums through AI reconstruction — blending digital archaeology and AI resurrection.

## 🎯 Why This Wins

**Resurrection Category**: Brings the dead web back to life:
- Wayback Machine snapshot retrieval and reconstruction
- AI-powered content inference and gap-filling
- Interactive conversations with "ghost" websites
- Semantic memory of resurrected content via vector embeddings
- Real-time resurrection pipeline from URL to living ghost

**Potential Value**: 
- **Digital Preservation**: Resurrect historical content with AI context
- **Education & Research**: Understand web history interactively
- **Entertainment**: Nostalgia + storytelling ("talk to 2000s websites")
- **Brand Resurrection as a Service**: SaaS for brands to revive legacy pages

**Creative Design**: Spooky "CRT flicker" terminal theme with interactive digital séance UI. Each resurrected site becomes a conversational ghost with personality based on its original content.

## 🪦 Resurrection Features

### 🔮 Mystical Resurrection Ritual
Transform boring loading screens into spectacular séances:
- **Rotating Pentagram** - Glowing green star with pulsing effects
- **Runic Circle** - 12 ancient symbols spinning in reverse
- **Floating Code Fragments** - Digital spirits (`<html>`, `{}`, `404`, `💀`)
- **Energy Pulses** - Expanding mystical waves
- **Cycling Messages** - "Summoning spirits from the Wayback Machine..."

### 💀 Digital Autopsy Report
Comprehensive post-mortem analysis of dead websites:
- **Vital Statistics** - Domain, era, age at death, last seen date, snapshot count
- **Tech Stack Detection** - Auto-identifies technologies (PHP, Flash, MySQL, etc.)
- **Cause of Death** - Intelligent analysis (Abandoned, Domain Expiration, Migration)
- **Spirit Essence** - Personality assessment with animated strength meter
- **Official Certification** - Rotating seal and examiner signature

### 👻 Ghost Interaction
- **AI-Powered Chat** - Converse with resurrected websites using LLM + historical data
- **Wayback Viewer** - Embedded iframe showing actual historical snapshots
- **Resurrection Gallery** - Browse previously summoned sites
- **Success Animation** - Triumphant celebration when resurrection completes

### 🎨 Gothic UI
- **CRT Terminal Theme** - Phosphor green on deep black
- **Scanline Effects** - Authentic retro monitor feel
- **Glitch Animations** - Title glitches and flickers
- **Floating Ghosts** - Animated emoji background
- **Pure CSS3** - All animations GPU-accelerated, 60fps, no libraries!

## 📚 Documentation

**New here?** Check out [INDEX.md](INDEX.md) for a complete guide to all documentation.

**Quick Links**:
- 🚀 [QUICKSTART.md](QUICKSTART.md) - Get running in 5 minutes
- 🏆 [SUBMISSION.md](SUBMISSION.md) - Hackathon submission
- 🎬 [HACKATHON_DEMO.md](HACKATHON_DEMO.md) - Demo script
- 🏗️ [ARCHITECTURE.md](ARCHITECTURE.md) - Technical deep dive
- ✅ [HACKATHON_CHECKLIST.md](HACKATHON_CHECKLIST.md) - Pre-submission checklist

## 🚀 Quick Start

### Local Development

```bash
# Install dependencies
npm install
cd frontend && npm install

# Set up environment variables
cp .env.example .env
# Add your OpenAI/Anthropic API key and Pinecone credentials

# Start backend resurrection engine
npm run backend

# Start frontend séance UI (separate terminal)
cd frontend
npm run dev

# Load Chrome extension (optional)
# 1. Open chrome://extensions/
# 2. Enable "Developer mode"
# 3. Click "Load unpacked"
# 4. Select the chrome_extension/ folder
```

### AWS Deployment

**Quick Deploy to AWS Amplify + App Runner:**

```bash
# 1. Push to GitHub
git push origin main

# 2. Deploy backend (AWS App Runner Console)
#    - Connect GitHub repo
#    - Use apprunner.yaml config
#    - Add environment variables

# 3. Deploy frontend (AWS Amplify Console)
#    - Connect GitHub repo
#    - Use amplify.yml config
#    - Add VITE_API_URL variable

# 4. Deploy infrastructure (optional - for S3/DynamoDB)
cd infrastructure
python -m venv venv
venv\Scripts\activate  # Windows
pip install -r requirements.txt
cdk bootstrap
cdk deploy
```

**📚 Deployment Guides:**
- [AMPLIFY_QUICKSTART.md](AMPLIFY_QUICKSTART.md) - Quick reference card
- [deploy-to-amplify.md](deploy-to-amplify.md) - Step-by-step guide
- [DEPLOYMENT.md](DEPLOYMENT.md) - Complete deployment documentation

**Alternative: Serverless (Lambda)**
```bash
npm install -g serverless
serverless deploy --stage prod
```

## 🏗️ Architecture

```
┌─────────────────────────────────────────────┐
│  Chrome Extension (Dead Link Detector)      │
│  - Detects 404s and dead sites             │
│  - "Summon This Page" button               │
│  - Sends URL to resurrection engine        │
└──────┬──────────────────────────────────────┘
       │
┌──────▼──────────────────────────────────────┐
│  React Frontend (Digital Séance UI)         │
│  - Gothic CRT terminal theme                │
│  - Ghost chat interface                     │
│  - Resurrection progress visualization      │
│  - Historical snapshot browser              │
└──────┬──────────────────────────────────────┘
       │
┌──────▼──────────────────────────────────────┐
│  FastAPI Backend (Resurrection Engine)      │
│  - Wayback Machine API integration          │
│  - LangChain orchestration                  │
│  - Semantic reconstruction pipeline         │
│  - Ghost personality generation             │
└──────┬──────────────────────────────────────┘
       │
┌──────▼──────────────────────────────────────┐
│  Data & AI Layer                            │
│  ├─ Wayback Machine: Historical snapshots   │
│  ├─ Pinecone: Vector memory storage         │
│  ├─ OpenAI/Anthropic: Ghost conversations   │
│  ├─ S3: Cached resurrection data            │
│  └─ DynamoDB: Resurrection metadata         │
└─────────────────────────────────────────────┘
```

## 🎮 User Experience

### The Resurrection Journey
1. **Enter URL** - Type any dead website (myspace.com, geocities.com, vine.co)
2. **Summon Ghost** - Click the glowing "👻 Summon" button
3. **Watch Ritual** - Pentagram spins, runes glow, code fragments float (10-15s)
4. **Celebrate Success** - Full-screen animation with checkmark and energy bursts (3s)
5. **Read Autopsy** - Comprehensive analysis with tech stack and cause of death
6. **Explore Ghost** - View the actual site in Wayback Machine iframe
7. **Chat with Spirit** - Ask questions about the site's history and content

### Key Features
- **One-Click Resurrection**: Enter any dead URL and watch it come back to life
- **Spectacular Animations**: Every loading screen is a mystical experience
- **Intelligent Analysis**: Auto-detects era, tech stack, and cause of death
- **Ghost Conversations**: Chat with resurrected sites about their history
- **Visual Time Travel**: Browse through historical snapshots
- **Personality Engine**: Each ghost has unique personality based on original content
- **Resurrection Gallery**: Browse previously summoned sites
- **Pure Performance**: All animations are CSS3, GPU-accelerated, 60fps

## 🎃 Kiro Features Demonstrated

- **Vibe Coding**: "Make this project more interesting" → Spectacular pentagram ritual and autopsy report generated in 30 minutes
- **Component Generation**: Created 3 complex React components with 1,200+ lines of CSS animations
- **Steering Rules**: Gothic-Archaeologist persona in `.kiro/steering/gothic_archeologist.md` ensures consistent narrative tone across all UI copy
- **Rapid Iteration**: From concept to working animations in minutes with AI assistance
- **Pure CSS Mastery**: Generated GPU-accelerated animations without any libraries
- **Documentation**: Auto-generated comprehensive guides, demo scripts, and testing instructions

## 📊 Demo Scenarios

### Best URLs to Try

**90s Era** (HTML 3.2, Frames, CGI):
- `geocities.com` - Classic 90s web with construction GIFs
- `angelfire.com` - Personal homepages galore
- `tripod.com` - Early web hosting

**2000s Era** (PHP, Flash, MySQL):
- `myspace.com` - Social network with custom profiles
- `friendster.com` - First major social network
- `digg.com` - Tech news aggregator

**2010s Era** (HTML5, CSS3, jQuery):
- `vine.co` - 6-second video platform (died 2017)
- `google.com/reader` - Beloved RSS reader

### What You'll See
1. **Pentagram Ritual** - 20s rotation, runic symbols, floating code
2. **Success Animation** - Growing circle, checkmark, energy bursts
3. **Autopsy Report** - Era badge, tech stack tags, death certificate
4. **Spirit Meter** - Animated progress bar showing ghost strength
5. **Resurrected Site** - Actual Wayback Machine snapshot

## 🏆 Hackathon Submission

- **Primary Category**: Resurrection (bringing dead websites back to life)
- **Bonus Categories**: 
  - Most Creative (interactive digital séance UI)
  - Best Startup Project (Brand Resurrection as a Service)
- **Video Demo**: [YouTube Link - 3min cinematic "séance" experience]
- **Live Demo**: [Deployment URL]
- **Chrome Extension**: Available in `/chrome_extension/`

## 🎥 Demo Video

[3-Minute Cinematic Demo - Coming Soon]

**Demo Highlights**:
- **Opening**: The problem of dead links and lost web history
- **Pentagram Ritual**: Watch the spectacular loading animation
- **Success Celebration**: Triumphant resurrection complete screen
- **Digital Autopsy**: Comprehensive analysis with tech detection
- **Ghost Interaction**: Converse with resurrected websites
- **Kiro Showcase**: How AI built the animations in 30 minutes

## 📸 Screenshots

### Resurrection Ritual
```
        ⚡ ANIMATED PENTAGRAM ⚡
              
         ᚠ    ᚢ    ᚦ    ᚨ
      ᚱ                      ᚲ
         ╱─────────────╲
    ᚷ  ╱   ⭐ STAR ⭐   ╲  ᚹ
      │                  │
    ᚺ  │   myspace.com   │  ᚾ
      │                  │
    ᛁ  ╲   "Summoning   ╱  ᛃ
         ╲   spirits..." ╱
         ᛇ    ᛈ    ᛉ    ᛊ
```

### Digital Autopsy Report
```
╔═══════════════════════════════════════╗
║  💀 DIGITAL AUTOPSY REPORT 💀         ║
║  Post-Mortem Analysis                 ║
╠═══════════════════════════════════════╣
║  📊 VITAL STATISTICS                  ║
║  Domain: myspace.com                  ║
║  Era: [Early 2000s Web]               ║
║  Age at Death: 15 years               ║
║  Last Seen: March 15, 2009            ║
║  Snapshots Preserved: 127             ║
║                                       ║
║  ⚙️ TECH STACK                        ║
║  [PHP 4] [MySQL] [Flash] [JS ES3]    ║
║                                       ║
║  🪦 CAUSE OF DEATH                    ║
║  Migration - Moved to new realm       ║
╚═══════════════════════════════════════╝
```

## 🏗️ Project Structure

```
aws-resurrection/
├── .kiro/                          # Kiro configuration
│   └── steering/                   # AI guidance
│       ├── aws-standards.md
│       └── gothic_archeologist.md  # Narrative tone rules
├── frontend/                       # React + Vite
│   ├── src/
│   │   ├── components/
│   │   │   ├── ResurrectionRitual.jsx    # Pentagram loading animation
│   │   │   ├── ResurrectionRitual.css    # Ritual styling (300+ lines)
│   │   │   ├── SiteAutopsy.jsx           # Autopsy report panel
│   │   │   ├── SiteAutopsy.css           # Autopsy styling (400+ lines)
│   │   │   ├── ResurrectionComplete.jsx  # Success animation
│   │   │   └── ResurrectionComplete.css  # Success styling (150+ lines)
│   │   ├── App.jsx                # Main app with state orchestration
│   │   └── App.css                # Gothic CRT terminal theme
│   └── package.json
├── infrastructure/                 # AWS CDK
│   ├── app.py
│   └── stacks/
├── backend-server.js              # Resurrection engine
├── .env.example                   # Configuration template
├── DEPLOYMENT.md                  # Setup guide
├── HACKATHON_DEMO.md             # Demo script
├── NEW_FEATURES.md               # Feature documentation
├── DEMO_SCRIPT.md                # Presentation guide
└── TESTING_GUIDE.md              # Testing instructions
```

## 🎨 Technical Highlights

### Animation Architecture
- **Pure CSS3** - No animation libraries, just creative keyframes
- **GPU-Accelerated** - All transforms and opacity changes
- **60fps Performance** - Optimized for smooth playback
- **Responsive Design** - Scales from mobile to desktop
- **SVG Graphics** - Pentagram and mystical symbols
- **Staggered Timing** - Choreographed reveals and transitions

### Component Design
- **ResurrectionRitual** - 5 animated layers (pentagram, runes, core, fragments, pulses)
- **SiteAutopsy** - 4 sections with intelligent data analysis
- **ResurrectionComplete** - Full-screen celebration with auto-dismiss
- **State Orchestration** - Smooth transitions between loading, success, and display

### Color Palette
- 🟢 **Phosphor Green** (#00FF41) - Primary, terminal authenticity
- 🔵 **Ghostly Blue** (#4ECDC4) - Secondary, ethereal accents
- 🔴 **Spectral Red** (#FF006E) - Autopsy, warnings, death
- 🟣 **Ethereal Purple** (#9D4EDD) - Mystical energy, magic
- ⚫ **Deep Black** (#0A0E27) - Void background

## 🤝 Contributing

This is a hackathon project, but contributions are welcome!

**Ideas for Enhancement**:
- 🔊 Add sound effects (ritual chanting, success chimes)
- 🎃 Easter eggs for famous dead sites (GeoCities, Angelfire)
- 📱 Enhanced mobile gestures
- 🤖 AI-powered historical analysis
- 📊 Timeline visualization
- 🎨 Additional themes (Cyberpunk, Steampunk)

**How to Contribute**:
1. Fork the repository
2. Create a feature branch
3. Follow the Gothic Archaeologist tone (see `.kiro/steering/`)
4. Submit a pull request

## 📄 License

MIT License - See LICENSE file

## 🙏 Acknowledgments

- **Wayback Machine** - For preserving internet history
- **Kiro** - For making AI-assisted development magical
- **The Dead Web** - For inspiring this digital séance

---

**Built with 💚 (phosphor green) for Kiroween Hackathon 2025**

*"The internet never forgets... it just needs a séance."* 👻
