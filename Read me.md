# BlocPrep Pro

**Gestion intelligent du bloc opératoire** — Application web pour brancardiers et équipes opératoires.

## 📋 Overview

BlocPrep Pro est une plateforme SaaS designed pour simplifier les opérations de bloc opératoire:
- **Checklists** - Vérifier rapidement les tables avant intervention
- **Planning** - Calendrier d'interventions avec gestion des blocs
- **Chat équipe** - Communication en temps réel
- **Interventions custom** - Créer des workflows personnalisés

**Stack:** HTML/CSS/JS (Frontend) + Supabase (Backend) + Vercel (Hosting)

---

## 🏗️ Architecture

```
blocprep-prod/
├── frontend/
│   ├── index.html           # Page principale
│   ├── styles/
│   │   └── index.css        # Design system (Refined Brutalism)
│   └── js/
│       ├── constants.js     # Data & interventions par défaut
│       ├── storage.js       # Gestion localStorage
│       ├── ui.js            # Rendu & DOM updates
│       └── app.js           # Logique applicative
├── backend/
│   ├── schema.sql           # Structure Supabase complète
│   ├── API.md               # Documentation API REST
│   ├── package.json         # Dépendances Node.js
│   └── .env.example         # Variables d'environnement
└── README.md               # Ce fichier
```

### Frontend - Modulaire & Maintenable

**Structure modulaire** avec séparation des concerns:
- **HTML** - Sémantique, accessible (WCAG 2.1 AA)
- **CSS** - Design tokens, variables CSS, responsive
- **JS** - 4 modules découplés
  - `constants.js` - Data par défaut (8 interventions)
  - `storage.js` - Persistance localStorage
  - `ui.js` - Rendu & interaction DOM
  - `app.js` - Orchestration & état global

**Tech:** Vanilla JS (no framework), CSS custom properties, localStorage

### Backend - Supabase + PostgreSQL

**Structure SQL** avec:
- 7 tables core + indexes + RLS policies
- Authentification JWT
- Views pré-calculées
- Triggers `updated_at` automatiques

**API REST** documentée:
- Endpoints CRUD pour chaque ressource
- Filtering, sorting, pagination
- Rate limiting (1000 req/min)
- Error handling structuré

---

## 🚀 Quick Start

### 1. Frontend (Local Development)

```bash
# Ouvre directement dans le navigateur
# Le fichier `frontend/index.html` fonctionne standalone
open frontend/index.html

# Ou avec un serveur local
cd frontend
python3 -m http.server 8000
# Puis: http://localhost:8000
```

**Mode offline:** Toutes les données sont sauvegardées en localStorage. Aucun backend requis pour le mode local.

### 2. Backend (Supabase)

#### Setup initial:
```bash
# 1. Crée un projet Supabase gratuit
# https://app.supabase.com/projects

# 2. Exécute le schema SQL
# Dans Supabase → SQL Editor → Colle le contenu de backend/schema.sql

# 3. Configure les variables d'environnement
cp backend/.env.example backend/.env
# Remplis: SUPABASE_URL, SUPABASE_ANON_KEY, SUPABASE_SERVICE_ROLE_KEY
```

#### Installation (optionnel, pour serverless functions):
```bash
cd backend
npm install
npm run dev
```

### 3. Déploiement (Vercel)

```bash
# Frontend sur Vercel (gratuit)
cd frontend
npm install -D vercel
vercel deploy

# Backend (Edge Functions) sur Vercel
cd backend
vercel deploy --prod
```

---

## 🎨 Design System

### Tokens

| Token | Valeur | Usage |
|-------|--------|-------|
| `--black` | `#0f0f0f` | Fond header, texte dominant |
| `--accent` | `#00d4aa` | Teal médical (CTA, highlights) |
| `--white` | `#ffffff` | Contenu blanc |
| `--gray-*` | Palette | Hiérarchie & subtleté |

### Typography

- **Display:** `Syne` 700-800 (titles) — géométrique, moderne
- **Mono:** `JetBrains Mono` (labels, code) — technical feel
- **Body:** System fonts (`-apple-system`, Segoe UI)

### Spacing

- Base unit: 8px
- Padding: 8px, 12px, 16px, 20px, 24px, 28px, 32px
- Gaps: 12px, 14px, 16px, 18px, 20px, 24px, 28px, 32px

### Interaction

- **Transitions:** `cubic-bezier(0.2, 0.8, 0.2, 1)` (300ms défaut)
- **Transforms:** scale, translateX, translateY (micro-animations)
- **Hover states:** underline scale, background shift, elevation
- **Active states:** color inversion, border highlight

---

## 📱 Responsive Design

### Breakpoints

- **Desktop:** 1200px+ (agenda 2-col)
- **Tablet:** 768px–1200px (agenda 1-col)
- **Mobile:** <768px (full-width, adjusted fonts)

### iPhone Optimization

- Full viewport utilization (`viewport-fit=cover`)
- Safe area support (navigation bars)
- Touch-friendly buttons (min 44px)
- Swipe gestures supportées (future)

---

## 🔐 Security & Compliance

### Frontend
- No API keys in code (utilise `SUPABASE_ANON_KEY` via env)
- localStorage encryption possible (future)
- CORS strict
- No external CDN (sauf Google Fonts)

### Backend (Supabase)
- **RLS enabled** - Row Level Security sur toutes les tables
- **JWT auth** - Tokens signés, expiry 7 jours
- **Role-based access** - admin, surgeon, nurse, porter
- **Rate limiting** - 1000 req/min/user
- **Encryption** - Password hashing bcrypt (Supabase built-in)

---

## 📊 Features by Tab

### ✓ Checklist
- Sélection intervention (8 types par défaut)
- Affichage tables + cochage
- Checkbox state persisté
- Visual feedback (strikethrough, color)

### 📅 Planning/Agenda
- Calendrier mois complet
- Navigation prev/next
- Sélection jour avec événements
- Formulaire d'ajout (date, heure, nom, bloc)
- Suppression événements
- Indicateur jours avec interventions

### 💬 Chat
- Messages temps réel (localStorage pour MVP)
- User ID aléatoire (future: from auth)
- Scroll auto au dernier message
- Entrée avec Enter

### ➕ Créer Intervention
- Formulaire: nom + items (un par ligne)
- Custom interventions sauvegardées
- Apparaît immédiatement dans la liste

---

## 🔄 Data Flow

```
User Action
  ↓
App.js (orchestration)
  ↓
Storage.js (localStorage save)
  ↓
UI.js (render update)
  ↓
DOM updated
  ↓
[Future: API sync to Supabase]
```

---

## 🚧 Roadmap

### Phase 1: MVP (✅ Done)
- [x] Frontend standalone offline
- [x] 8 interventions par défaut
- [x] Checklists + Planning + Chat
- [x] Design Refined Brutalism
- [x] localStorage persistence

### Phase 2: Backend (Next)
- [ ] Supabase auth (Google/SSO)
- [ ] API REST sync
- [ ] Multi-user support
- [ ] Real-time chat (WebSocket)
- [ ] Analytics tracking

### Phase 3: SaaS (Q4 2026)
- [ ] Team management
- [ ] Hospital admin dashboard
- [ ] Billing & subscriptions
- [ ] Mobile app (React Native)
- [ ] Offline-first sync

### Phase 4: Enterprise (2027)
- [ ] Custom HIPAA compliance
- [ ] Integration HL7/FHIR
- [ ] Advanced reporting
- [ ] ML recommendations
- [ ] API marketplace

---

## 📞 Support & Contact

**Hospital:** Hôpital privé des Peupliers (Ramsay Santé)  
**Built for:** Brancardiers & équipes opératoires  
**Questions:** support@blocprep.fr  
**Feedback:** https://forms.gle/blocprep-feedback

---

## 📄 License

MIT — Libre d'usage & modification

---

## 🙏 Credits

- **Design:** Refined Brutalism + Medical Tech aesthetic
- **Tech:** Vanilla JS, Supabase, Vercel
- **Data:** 8 interventions réelles du bloc opératoire

---

## 🔗 Links

- **Live:** https://blocprep-pro.vercel.app
- **GitHub:** https://github.com/medhi667/blocprep-pro
- **Supabase:** https://app.supabase.com
- **API Docs:** [backend/API.md](backend/API.md)
- **Schema:** [backend/schema.sql](backend/schema.sql)

---

**v1.0.0** — Built with ❤️ by Médhi for surgical teams.
