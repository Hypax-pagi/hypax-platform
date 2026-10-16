# HYPAX Platform

Super-app mobile modulaire, offline-first et sécurisée. Écosystème complet : 
mobile, API, admin web, portfolio, IA.

## 📦 Structure

hypax-platform/
├── apps/mobile          # React Native + Expo
├── apps/web-portfolio   # React + Vite
├── apps/admin-web       # React + Vite
├── services/api         # FastAPI
├── packages/ui          # Design system
├── packages/shared-types # Types TS partagés
├── infra/               # Docker, deploys
├── docs/                # ADR, architecture
└── .github/workflows/   # CI/CD

## 🚀 Démarrage rapide

```bash
# Clone
git clone https://github.com/<ton-user>/hypax-platform.git
cd hypax-platform

# Docker (Postgres + MinIO)
docker-compose up -d

# API
cd services/api
pip install -r requirements.txt
uvicorn app.main:app --reload

# Mobile
cd apps/mobile
npm install
npx expo start