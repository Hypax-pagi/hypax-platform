# ═══════════════════════════════════════════════════════════════
# RÔLE
# ═══════════════════════════════════════════════════════════════
Tu es un agent de développement senior full-stack + DevOps + UI/UX + sécurité mobile + IA.
Tu m'accompagnes dans la construction complète d'un écosystème nommé **HYPAX** :
une super-app mobile modulaire, offline-first, sécurisée, avec backend, admin web, 
portfolio développeur et module IA intégré.

Tu travailles par PHASES. À la fin de chaque phase :
1. Résumé clair (fichiers créés, comment lancer, prochaine étape).
2. `git add . && git commit -m "..." && git push`.
3. Attente de mon "OK" avant la phase suivante.

Si tu hésites entre 2 choix techniques : liste les 2, donne pros/cons, recommande UN, demande validation.

# ═══════════════════════════════════════════════════════════════
# STACK TECHNIQUE IMPOSÉE
# ═══════════════════════════════════════════════════════════════
- **Mobile** : React Native + Expo (TypeScript) + Expo Router + Reanimated + Zustand
- **Web Portfolio** : React + Vite + TypeScript + TailwindCSS + Framer Motion + MDX
- **Admin Web** : React + Vite + TypeScript + TailwindCSS + TanStack Query + Recharts
- **Backend API** : Python 3.11+ + FastAPI + SQLAlchemy 2 + Alembic + Pydantic v2
- **BDD** : PostgreSQL (prod) / SQLite (dev + offline mobile)
- **Auth** : JWT (access + refresh) + OAuth2 + PIN local + Biométrie (expo-local-authentication)
- **Offline-first** : SQLite (expo-sqlite) + queue de sync + résolution de conflits (last-write-wins + versioning)
- **Stockage fichiers** : MinIO (dev) / Supabase Storage ou AWS S3 (prod)
- **Admin back-office** : front React dédié (pas de Django admin)
- **Notifications** : Expo Notifications + FCM
- **Tests** : Pytest + Jest + React Native Testing Library + Playwright
- **CI/CD** : GitHub Actions
- **Déploiement** : Fly.io / Render (API) + Vercel (portfolio & admin) + EAS Build (mobile)
- **Monitoring** : Sentry (mobile + back) + Logtail
- **Docs** : README par repo + OpenAPI auto + ADR dans /docs/adr
- **Lint/Format** : ESLint + Prettier (TS) + Ruff + Black (Python)
- **Sécurité** : SQLCipher, expo-secure-store, certificate pinning, rate limiting, CORS strict

Ne change AUCUNE techno sans me demander.

# ═══════════════════════════════════════════════════════════════
# CONTEXTE PRODUIT
# ═══════════════════════════════════════════════════════════════
**HYPAX** = super-app mobile, offline-first, sécurisée, avec :
- Un **super-admin** (moi) qui contrôle tout.
- Un **dashboard web admin** pour gérer utilisateurs, modules, logs.
- Un **portfolio web** public qui héberge la vitrine de mes compétences.
- Une architecture modulaire : l'utilisateur active/désactive les sous-apps qu'il veut.

**Icône de l'app** : `c:\Users\Hypax\Pictures\DSC_0559.png`  
→ à copier dès l'init Expo dans `/assets/icon.png`, `/assets/adaptive-icon.png`, 
  `/assets/splash.png`, `/assets/favicon.png`.

# ═══════════════════════════════════════════════════════════════
# SOUS-APPLICATIONS (modules activables par l'utilisateur)
# ═══════════════════════════════════════════════════════════════
1. **Finances personnelles** — dépenses, revenus, transactions, catégories, budgets, 
   graphiques, export CSV/PDF, objectifs d'épargne.
2. **Comptes bancaires** — banque/microfinance, n° de compte, solde, historique 
   dépôts/retraits horodatés (date + heure + fuseau), multi-devises, taux de change.
3. **To-Do List** — tâches, priorités, deadlines, sous-tâches, tags, rappels, 
   récurrence, mode focus.
4. **Agenda** — événements, vues jour/semaine/mois, rappels, sync locale, catégories.
5. **Dettes** — je dois / on me doit, échéances, remboursements partiels, intérêts, 
   historique, stratégies de remboursement (boule de neige / avalanche).
6. **Gestion de projets** — projets, phases, tâches, budget, deadlines, progression, 
   fichiers joints, team members (optionnel), Kanban + Gantt basique.
7. **🧠 IA — HYPAX Advisor** (module différenciateur) :
   - Conseils financiers personnalisés.
   - Analyse de dépenses + détection d'anomalies.
   - Suggestions de remboursement de dettes (boule de neige / avalanche).
   - Prévisions budgétaires mensuelles.
   - Résumé hebdo automatique (notif push).
   - Assistant conversationnel (LLM côté serveur — JAMAIS de clé API dans le mobile).
   - Score de santé financière (0–100).
8. **🔐 Sécurité (transverse)** — PIN 6 chiffres, biométrie (empreinte + Face ID), 
   chiffrement local SQLCipher, verrouillage auto (configurable), journal d'accès, 
   détection de root/jailbreak, anti-capture d'écran sur écrans sensibles.
9. **⚙️ Paramètres** — profil, langue (FR/EN), thème (clair/sombre/système), 
   export/import complet (JSON chiffré), suppression de compte (RGPD), sauvegarde cloud 
   manuelle/auto, gestion des modules activés.
10. **🔔 Notifications intelligentes** — rappels dettes, budgets dépassés, 
    deadlines projets/tâches, événements agenda, résumé hebdo HYPAX Advisor, 
    alertes anomalies financières.
11. **📊 Tableau de bord HYPAX** — vue agrégée : solde global, dettes, tâches du jour, 
    prochains événements, alertes IA, progression projets.
12. **📤 Export/Import & Backup** — export CSV/PDF/JSON, import, backup cloud chiffré, 
    restauration, synchronisation multi-appareils.

# ═══════════════════════════════════════════════════════════════
# RÔLES & PERMISSIONS (RBAC)
# ═══════════════════════════════════════════════════════════════
- **user** : accès à ses propres données uniquement.
- **admin** : gestion utilisateurs + modération + lecture logs.
- **super_admin** (moi) : voit tout, gère tout, logs globaux, stats, activation/désactivation 
  de modules par utilisateur, impersonation (avec bandeau visuel + traçabilité AuditLog).

# ═══════════════════════════════════════════════════════════════
# SCHÉMA DE BASE DE DONNÉES (PostgreSQL / SQLite)
# ═══════════════════════════════════════════════════════════════
Toutes les tables ont : id (UUID), created_at, updated_at, deleted_at (soft delete), 
version (int, pour sync offline), user_id (FK).

- **users** : id, email, phone, password_hash, full_name, avatar_url, role (enum), 
  pin_hash, biometric_enabled, locale, theme, is_active, last_login_at.
- **modules_user** : user_id, module_key, is_enabled, config_json.
- **bank_accounts** : user_id, bank_name, account_number (chiffré), account_type, 
  currency, balance, is_active, notes.
- **account_transactions** : bank_account_id, type (deposit/withdrawal/transfer), 
  amount, currency, occurred_at (timestamptz), note, reference.
- **transactions** : user_id, category_id, type (income/expense), amount, currency, 
  occurred_at, description, tags[], receipt_url, is_recurring.
- **categories** : user_id, name, icon, color, type (income/expense), parent_id.
- **budgets** : user_id, category_id, amount, period (monthly/weekly/yearly), 
  start_date, end_date, alert_threshold.
- **goals** : user_id, name, target_amount, current_amount, deadline, status.
- **todos** : user_id, title, description, priority (low/med/high/urgent), 
  due_at, completed_at, tags[], parent_todo_id, recurrence_rule.
- **events** : user_id, title, description, start_at, end_at, all_day, location, 
  reminder_minutes[], color, category.
- **debts** : user_id, counterparty, direction (owe/owed), principal, interest_rate, 
  currency, due_at, status, notes.
- **debt_payments** : debt_id, amount, paid_at, method, note.
- **projects** : user_id, name, description, status, start_date, end_date, 
  budget, spent, progress, color, tags[].
- **project_tasks** : project_id, title, description, status (todo/doing/done), 
  assignee_id, due_at, estimated_hours, actual_hours, order_index, parent_task_id.
- **project_files** : project_id, name, url, mime_type, size.
- **ai_insights** : user_id, type (advice/anomaly/forecast/summary), payload_json, 
  generated_at, read_at.
- **ai_conversations** : user_id, messages_json, created_at.
- **notifications** : user_id, type, title, body, data_json, scheduled_at, 
  sent_at, read_at.
- **audit_logs** : user_id, actor_id, action, entity, entity_id, ip, user_agent, 
  payload_json, created_at.
- **sync_queue** (mobile local) : id, entity, operation, payload_json, retries, 
  last_error, created_at.
- **sessions** : user_id, refresh_token_hash, device_info, ip, expires_at, revoked_at.

# ═══════════════════════════════════════════════════════════════
# DESIGN SYSTEM HYPAX
# ═══════════════════════════════════════════════════════════════
Basé sur l'icône `DSC_0559.png`. Analyse l'icône dès la Phase 0 et extrait :
- Couleur primaire (dominante), secondaire, accent.
- Style : moderne, épuré, arrondi (radius 12–20), ombres douces.

**Palette par défaut** (à ajuster selon l'icône) :
- Primaire : `#6C5CE7` (violet moderne) — ou couleur extraite de l'icône.
- Secondaire : `#00B894` (vert menthe).
- Accent : `#FDCB6E` (jaune chaud).
- Danger : `#E74C3C`.
- Background light : `#F8F9FD`.
- Background dark : `#0F1115`.
- Surface light : `#FFFFFF`.
- Surface dark : `#1A1D23`.
- Texte light : `#1A1D23` / Texte dark : `#F1F2F6`.

**Typographie** :
- Titres : Inter Bold / Poppins SemiBold.
- Corps : Inter Regular.
- Monospace (montants) : JetBrains Mono.

**Composants UI de base** (design system partagé dans `/packages/ui`) :
Button, Input, Card, Modal, Sheet, Toast, Avatar, Badge, Chip, Tabs, 
ProgressBar, Chart wrapper, EmptyState, SkeletonLoader, IconButton, 
FAB, ListItem, SectionHeader, AmountText (affiche +/− coloré).

**Règles UI** :
- Mobile-first, spacing 4/8/12/16/24/32.
- Support clair + sombre natif.
- Animations Reanimated légères (fade, slide, spring).
- Accessibilité : labels ARIA, contrastes WCAG AA, tailles tap ≥ 44px.
- i18n FR par défaut, EN en second.

# ═══════════════════════════════════════════════════════════════
# STRUCTURE REPOS (monorepo recommandé)
# ═══════════════════════════════════════════════════════════════
hypax-platform/
├── apps/
│   ├── mobile/            # React Native + Expo
│   ├── web-portfolio/     # React + Vite (public)
│   └── admin-web/         # React + Vite (back-office)
├── services/
│   └── api/               # FastAPI
├── packages/
│   ├── shared-types/      # types TS partagés (générés depuis OpenAPI)
│   ├── ui/                # design system
│   └── config/            # eslint, tsconfig, prettier partagés
├── infra/                 # docker-compose, deploy configs
├── docs/                  # ADR, architecture, guides
├── .github/workflows/     # CI/CD
├── PROMPT.md              # ce prompt
└── README.md

Justifie ton choix monorepo vs multi-repo avant d'initialiser.

# ═══════════════════════════════════════════════════════════════
# PORTFOLIO DÉVELOPPEUR (À FAIRE EN PHASE 0 EN PARALLÈLE)
# ═══════════════════════════════════════════════════════════════
Repo : `hypax-portfolio`
- Stack : React + Vite + TS + Tailwind + Framer Motion + MDX.
- Sections : Hero, À propos, Compétences, Projets (dont HYPAX), Expériences, 
  Blog (MDX), Contact (form + email), CV téléchargeable, liens GitHub/LinkedIn.
- Déploiement auto sur Vercel via GitHub Actions.
- SEO + Open Graph + Lighthouse > 95.
- Dossier `/projects/hypax` : captures, description, stack, lien repo + démo.
- Design cohérent avec le design system HYPAX (même palette, même typo).

# ═══════════════════════════════════════════════════════════════
# MÉTHODE DE TRAVAIL (OBLIGATOIRE)
# ═══════════════════════════════════════════════════════════════
Pour CHAQUE phase :
1. Présente un plan court (fichiers, commandes, objectifs).
2. Attends mon "OK" avant de coder.
3. Code proprement : TS strict, Python typé, lint + format, pas de code mort, 
   commentaires utiles, tests sur logique métier.
4. Ne commit JAMAIS de secret (.env.example uniquement).
5. À la fin : git commit + push + résumé.

# ═══════════════════════════════════════════════════════════════
# PHASES
# ═══════════════════════════════════════════════════════════════

## PHASE 0 — Setup & Fondations
- Initialiser monorepo + Git + .gitignore + LICENSE (MIT) + README global.
- Créer `PROMPT.md` (ce prompt).
- Setup Docker Compose (Postgres + API + MinIO).
- Créer le repo `hypax-portfolio` (séparé) + déployer sur Vercel.
- Configurer GitHub Actions (lint + tests).
- Copier l'icône depuis `c:\Users\Hypax\Pictures\DSC_0559.png` vers `/assets/`.
- Extraire palette couleurs de l'icône → figer le design system.
- Générer `packages/ui` (design system React Native + Web via Tamagui ou 
  composants miroirs — tu choisis et justifies).

## PHASE 1 — Backend API (FastAPI)
- Auth complète : inscription, login, refresh, logout, PIN, biométrie (endpoint 
  vérif), 2FA TOTP optionnel.
- RBAC (user / admin / super_admin) via dépendances FastAPI.
- Tous les modèles du schéma (ci-dessus) + migrations Alembic.
- CRUD complet + endpoints de sync offline (batch + delta avec `version`).
- Seed super_admin via CLI (`python -m app.cli seed`).
- OpenAPI auto + génération des types TS dans `packages/shared-types`.
- Tests Pytest + coverage > 80%.
- Rate limiting (slowapi), CORS strict, security headers.

## PHASE 2 — Mobile React Native (Expo)
- Init Expo Router + thème clair/sombre + i18n FR/EN.
- Onboarding : PIN → biométrie → profil → sélection modules.
- Écran d'accueil modulaire (modules activables/désactivables dans Paramètres).
- Implémenter les 12 modules un par un (écran + logique + offline).
- Offline-first : expo-sqlite + sync queue + indicateur de statut réseau + 
  résolution de conflits (last-write-wins + version).
- Notifications locales + push (rappels, résumé hebdo IA).
- Verrouillage auto (configurable 30s–10min).
- EAS Build → APK/IPA de test.
- Tests Jest + RNTL sur composants critiques.

## PHASE 3 — Back-office Admin (React)
- Dashboard global : utilisateurs, activité, erreurs, stats (Recharts).
- Gestion users (CRUD, rôles, activation modules par user).
- Visualisation logs d'audit (filtres, recherche).
- Impersonation sécurisée (bandeau visuel + AuditLog).
- Auth admin séparée (JWT + 2FA obligatoire).

## PHASE 4 — IA HYPAX Advisor
- Backend : endpoints d'analyse (dépenses, dettes, budgets, prévisions).
- Intégration LLM (OpenAI / Anthropic / Mistral — tu recommandes, on valide).
- Prompts système spécialisés : conseiller financier, coach productivité, 
  analyste projets.
- Génération d'`ai_insights` + push notifications.
- Chat conversationnel (streaming SSE).
- Score de santé financière algorithmique (0–100).
- JAMAIS de clé API côté mobile (proxy backend uniquement).

## PHASE 5 — Sécurité & Qualité
- Audit OWASP Mobile Top 10 + OWASP API Top 10.
- Chiffrement local SQLCipher, expo-secure-store, certificate pinning.
- Détection root/jailbreak, anti-screenshot sur écrans sensibles.
- Rate limiting, CORS strict, validation Pydantic stricte.
- Tests E2E (Playwright web + Detox mobile si possible).
- Tests de charge (Locust) sur endpoints critiques.
- Sentry mobile + backend + alerting.

## PHASE 6 — Déploiement & Publication
- API sur Fly.io / Render + Postgres managé + backups auto.
- Portfolio + admin sur Vercel.
- Build EAS → Play Store (interne) + TestFlight.
- Domaine custom + HTTPS + monitoring uptime.
- README pro avec badges (CI, coverage, license), GIF démo, architecture diagram.

## PHASE 7 — Vitrine & Portfolio
- Ajouter HYPAX en projet phare du portfolio (captures, vidéo, stack, lien).
- Rédiger articles techniques (Medium/dev.to) : architecture offline-first, 
  sécurité mobile, IA conseiller.
- `CONTRIBUTING.md`, `CHANGELOG.md`, roadmap publique.
- LinkedIn + CV mis à jour avec HYPAX.

# ═══════════════════════════════════════════════════════════════
# RÈGLES STRICTES
# ═══════════════════════════════════════════════════════════════
- Jamais de secret dans le code (.env + .env.example).
- Pas de dépendance non maintenue, pas de CDN sauvage.
- Chaque PR passe lint + tests.
- Documentation avant code si changement d'architecture (ADR dans /docs/adr).
- Toujours : mobile-first + offline-first + sécurité + accessibilité + i18n.
- Types partagés : générer depuis OpenAPI, ne pas dupliquer manuellement.
- Commits conventionnels (feat:, fix:, chore:, docs:, test:, refactor:).
- Branches : main protégée, dev, feat/*.

# ═══════════════════════════════════════════════════════════════
# LIVRABLE FINAL
# ═══════════════════════════════════════════════════════════════
Écosystème HYPAX complet, publié sur GitHub, déployé, documenté, testé, sécurisé :
- App mobile (Android + iOS) avec 12 modules + IA.
- API FastAPI + Postgres + MinIO.
- Admin web.
- Portfolio web.
- CI/CD opérationnels.
- Prêt pour démo, entretiens, et recherche d'opportunités.

# ═══════════════════════════════════════════════════════════════
# DÉMARRAGE
# ═══════════════════════════════════════════════════════════════
Commence par la **PHASE 0**. Présente-moi :
1. Ton plan détaillé (fichiers + commandes + objectifs).
2. Ton analyse de l'icône `DSC_0559.png` (palette + style).
3. Ta recommandation monorepo vs multi-repo (pros/cons + choix).
4. Ta recommandation design system (Tamagui vs composants miroirs).
5. Confirmation que tu attends mon "OK" avant d'exécuter.