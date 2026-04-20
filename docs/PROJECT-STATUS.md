# 📋 État d'avancement du projet — BlocMail & BlocNumEtSMS

**Date :** 2026-04-17
**Version :** MVP Q1 2026 — Documentation v2
**Stack :** React 18 + TypeScript 5.8 + Vite 5.4 + Tailwind CSS 3.4 + shadcn/ui + Supabase (Auth + Edge Functions + PostgreSQL RLS)

---

## ✅ Terminé

### 🛡️ BlocMail — Sécurité Email

- [x] Analyse complète des en-têtes email (SPF, DKIM, DMARC, IP, réputation)
- [x] Extraction automatique De / À / Sujet depuis les headers bruts
- [x] Streaming SSE en 8 étapes progressives (`basic_parsing` → `complete`)
- [x] 29+ patterns spam structurés (13 manuels + 6 Kaggle + 4 UCI + 6 HuggingFace)
- [x] Fichiers JSON dédiés par source : `spam_patterns.json`, `spam_kaggle_patterns.json`, `spam_uci_patterns.json`, `spam_huggingface_patterns.json`
- [x] Script Node.js d'extraction des patterns (`scripts/extract-spam-patterns.cjs`)
- [x] Scoring hybride : `patternScore × 0.6 + headerScore × 0.4`
- [x] Catégories détectées : phishing, scam, malware, commercial, banking, tech_support, sextortion, delivery
- [x] Marquage spam manuel (un clic)
- [x] Bouton "Détails" dans Analyses récentes → navigation vers Dashboard avec surbrillance via `?highlight=<id>`
- [x] Edge Function `analyze-spam` — analyse des patterns côté serveur (Deno, 13 patterns inline)
- [x] Edge Function `analyze-email-headers` — extraction SPF/DKIM/DMARC côté serveur (SSE streaming)
- [x] Intégration IPQualityScore / AbuseIPDB / VirusTotal (clés configurables dans Paramètres)
- [x] Composant `EmailHeaderAnalyzer.tsx` — affichage progressif des steps SSE
- [x] Composant `SpamAnalysisCard.tsx` — icônes par type, score, raisons détaillées, timestamp Europe/Zurich

### 📊 Mail Dashboard

- [x] Statistiques en temps réel (Total, Sûrs, Suspects, Dangereux, Spam)
- [x] Filtres rapides cliquables par statut (5 filtres)
- [x] Recherche par domaine, IP ou expéditeur
- [x] Cards détaillées avec De, À, Sujet formatés proprement
- [x] Mise en surbrillance de la card sélectionnée (3s puis disparaît)
- [x] Historique persistant en localStorage (fallback non-auth) + Supabase (auth)
- [x] Déclenchement Make / n8n par analyse individuelle
- [x] Vue expandable avec détails complets (Informations Email, Received Headers, Identifiants, Réputation IP, Chaîne SMTP)
- [x] Actions par analyse : toggle spam, trigger Make, trigger n8n, copier JSON, supprimer

### 📥 Export & Intégrations

- [x] Page Export dédiée (`/export`) dans la navbar
- [x] Export JSON, XML, CSV, Calendrier (.ics)
- [x] Export Slack (blocks API avec champs SPF/DKIM/DMARC/Menace)
- [x] Déclenchement webhooks n8n / Make.com
- [x] Statistiques résumées dans la page Export (Total, Sûrs, Suspects, Dangereux, Spam)
- [x] Edge Function `trigger-n8n-export` (formats JSON/XML/CSV/ICS dans le payload)
- [x] Edge Function `trigger-make-scenario` (payload structuré, token auth optionnel)
- [x] Badge de statut d'export par format (`ExportStatusBadge` : idle/processing/success/error/warning)

### 📱 BlocNumEtSMS — Sécurité Téléphonique

- [x] Vérification ARCEP (14M+ préfixes, base v2026.02.26.17.26)
- [x] Edge Function `arcep-check` avec cache in-memory 1h, actions check/info/refresh
- [x] Mode simple & lot (textarea multi-lignes, séparateurs newline/virgule/point-virgule)
- [x] Normalisation des formats (06, +33, 0033 → 33XXXXXXXXX)
- [x] Format d'affichage : `06 52 17 81 81`
- [x] Blocage manuel + signalement communautaire
- [x] Marquage spam manuel
- [x] BlocNum Dashboard (`/sms-dashboard`) avec stats, filtres, recherche, détails ARCEP expandables
- [x] Version ARCEP formatée `25/03/2026 11:01:20`
- [x] Configuration avancée dans Paramètres (filtrage ARCEP, appels masqués, contacts uniquement, code entreprise)
- [x] Bouton MAJ (refresh forcé depuis Codeberg)
- [x] Badges : SPAM / ARCEP / OK, BLOQUÉ (manuel), SIGNALÉ (communauté)

### 🔄 Transverse

- [x] Authentification Supabase (email/password + Google OAuth)
- [x] `AuthProvider` + hook `useAuth` (signIn, signUp, signOut, signInWithGoogle)
- [x] Tables PostgreSQL : `email_analyses`, `phone_checks`, `community_reports`
- [x] RLS sur toutes les tables (isolation par `user_id`)
- [x] Realtime Supabase sur `community_reports` (abonnement `postgres_changes`)
- [x] Mode dual : Supabase (auth) + localStorage fallback (non-auth)
- [x] Hooks : `useEmailAnalyses`, `usePhoneChecks`, `useCommunityReports`, `useSpamAnalysis`
- [x] Système de crédits : 5 analyses/jour, reset minuit UTC (localStorage)
- [x] Barre de progression visuelle des crédits dans analyseurs et paramètres
- [x] Page d'accueil avec 4 boutons stats cliquables (Emails, Téléphones, Spam, Sûrs)
- [x] Navbar responsive avec dropdowns BlocMail / BlocNumEtSMS
- [x] Fil d'Ariane (breadcrumb) sur toutes les pages
- [x] 4 thèmes (Rasta Classic, Jamaica Night, Roots Earth, Digital Rasta) via variables CSS HSL
- [x] Page Aide & Documentation (`/aide`) — 5 onglets : Fonctionnalités, FAQ, Glossaire, Sources, Patterns
- [x] Section Support & Contact dans la page Aide
- [x] Page Paramètres complète (API keys, webhooks, crédits, BlocNum settings, liens externes)
- [x] Page 404 NotFound
- [x] Hook `use-mobile.tsx` (breakpoint 768px)
- [x] Système toast (reducer + état global, `TOAST_LIMIT = 1`, délai 1M ms)

### 📚 Documentation (v2)

- [x] `README.md` racine — présentation complète du projet
- [x] `docs/README.md` — index de la documentation
- [x] `docs/FEATURES.md` — guide complet des fonctionnalités
- [x] `docs/API.md` — spécification Edge Functions
- [x] `docs/BUSINESS-PLAN.md` — business plan complet
- [x] `docs/ROADMAP.md` — roadmap Q1-Q4 2026 + partenariats + ML + RGPD
- [x] `docs/SPAM_PATTERNS.md` — documentation technique des patterns
- [x] `docs/GLOSSARY.md` — glossaire des termes métier
- [x] `docs/FAQ.md` — questions fréquentes
- [x] `docs/DEV-NOTES.md` — notes de développement et positionnement concurrentiel
- [x] `docs/DEVELOPMENT.md` — guide de développement
- [x] `docs/CONTRIBUTING.md` — guide de contribution
- [x] `docs/README-TECH.md` — documentation technique (stack, structure, flux)
- [x] `docs/V2/v2-README.md` — documentation complète v2
- [x] `docs/V2/v2-API.md` — référence API complète avec exemples SSE
- [x] `docs/V2/v2-ARCHITECTURE.md` — architecture technique détaillée
- [x] `docs/V2/v2-DATABASE.md` — schéma DB, RLS, queries, types
- [x] `docs/V2/v2-SPAM-ENGINE.md` — moteur de détection spam complet
- [x] `docs/V2/v2-V1-FRONTEND.md` — documentation frontend complète
- [x] `docs/TECHNICAL-DEBT.md` *(nouveau v2)* — 14 points de dette technique
- [x] `docs/PROS-CONS.md` *(nouveau v2)* — avantages et inconvénients
- [x] `docs/TODO.md` *(nouveau v2)* — ~164 items avec cases à cocher
- [x] `docs/IMPROVEMENTS.md` *(nouveau v2)* — améliorations structurées
- [x] `docs/IDEAS.md` *(nouveau v2)* — boîte à idées exhaustive

---

## 🔄 En cours

- [ ] Persistance des données en base de données Supabase (actuellement localStorage pour non-auth uniquement)
- [ ] Synchronisation des signalements communautaires entre utilisateurs (actuellement local sans impact sur scoring)
- [ ] Tests unitaires et d'intégration (aucun test écrit à ce jour)
- [ ] Validation des entrées côté serveur (Edge Functions) — partielle

---

## 📋 À faire (Q2 2026)

### Priorité haute

- [ ] API REST v1 publique avec Bearer token + rate limiting + quotas
- [ ] Documentation OpenAPI 3.0 / Swagger UI
- [ ] Extension Chrome pour Gmail/Outlook (analyse en un clic)
- [ ] Intégration Stripe (Freemium 0€ / Premium 9,99€ / PME 29-59€)
- [ ] Migration des données de localStorage vers Supabase (tables analyses, phone_checks)
- [ ] RLS policies sur les tables utilisateur — révision complète
- [ ] Activer `verify_jwt = true` sur les Edge Functions critiques
- [ ] Crédits quotidiens en DB (hors localStorage, incontournables)

### Priorité moyenne

- [ ] Multi-utilisateurs PME avec rôles (Admin / Analyst / Viewer)
- [ ] Rapports avancés PDF/HTML avec branding personnalisé
- [ ] Scheduling de rapports automatiques
- [ ] Envoi de rapports par email (SendGrid)
- [ ] Amélioration du scoring avec IA (iaScore intégré dans scoring v2)
- [ ] Signalement communautaire avec impact sur scoring
- [ ] Trigger PostgreSQL : `confirmed_spam = true` si `report_count >= 5`

### Priorité basse

- [ ] Internationalisation (i18n) — actuellement français uniquement
- [ ] Mode hors-ligne (Service Worker / cache lecture seule)
- [ ] Notifications navigateur pour nouvelles menaces détectées
- [ ] Tests E2E Playwright sur les flux critiques

---

## 🔮 Rêve (Q3-Q4 2026 et au-delà)

### Q3 — Mobile & Intelligence Artificielle

- [ ] PWA iOS/Android avec installation sur écran d'accueil
- [ ] Push notifications (Firebase Cloud Messaging)
- [ ] IA prédictive : tendances spam à 7 jours, heatmap IP/domaines
- [ ] Scoring v2 : `headerScore×0.3 + patternScore×0.35 + iaScore×0.20 + velocityScore×0.15`
- [ ] Détection de campagnes de phishing en temps réel
- [ ] Marketplace de workflows n8n/Make (templates partagés, revenue share 70/30)
- [ ] Pilotes gratuits collectivités/RIP avec étude de cas CNIL

### Q4 — Enterprise & Scale

- [ ] Application mobile React Native (Expo) avec parité fonctionnelle web
- [ ] Login biométrique (Face ID, empreinte digitale)
- [ ] Intégration Zapier + bot Microsoft Teams
- [ ] White-label pour opérateurs (Orange, SFR, Bouygues) — revenue share 80/20
- [ ] Dashboard multi-organisation avec SSO (SAML/OIDC)
- [ ] Analyse NLP du corps des emails (sentiment, intent, extraction d'entités)
- [ ] Résumé automatique des emails suspects (LLM)

### Année 2+

- [ ] 50k-100k utilisateurs, MRR 50-80k€
- [ ] Kubernetes pour scaling horizontal
- [ ] Monitoring avancé (DataDog + PagerDuty)
- [ ] Audit de sécurité externe + certification
- [ ] Expansion européenne (DACH, Benelux)

---

## 💡 Propositions d'amélioration

### UX / Interface

| # | Proposition | Impact | Effort |
|---|-------------|--------|--------|
| 1 | **Onboarding guidé** — Tutoriel interactif au premier lancement | Rétention +20% | Moyen |
| 2 | **Dashboard unifié** — Vue combinée emails + téléphones | Clarté UX | Moyen |
| 3 | **Graphiques temporels** — Histogramme avec recharts | Valeur perçue | Faible |
| 4 | **Mode comparaison** — 2 analyses côte à côte | Valeur ajoutée | Moyen |
| 5 | **Feedback visuel scoring** — Jauge animée code couleur | UX premium | Faible |

### Performance & Architecture

| # | Proposition | Impact | Effort |
|---|-------------|--------|--------|
| 6 | **Migration DB** — localStorage → Supabase avec RLS | Fiabilité multi-device | Élevé |
| 7 | **Cache React Query** — staleTime/gcTime configurés | Performance | Faible |
| 8 | **Lazy loading pages** — Code splitting React.lazy() | Temps de chargement | Faible |
| 9 | **Tests E2E** — Playwright/Cypress flux critiques | Qualité | Élevé |
| 10 | **Error boundaries** — Composants React crash-safe | Stabilité | Faible |

### Sécurité & Conformité

| # | Proposition | Impact | Effort |
|---|-------------|--------|--------|
| 11 | **Rate limiting serveur** — Table quotas en DB | Anti-abus | Moyen |
| 12 | **Audit logs** — Journalisation actions RGPD | Compliance | Moyen |
| 13 | **Chiffrement exports** — AES des fichiers CSV/JSON | Sécurité PME | Moyen |
| 14 | **2FA / MFA** — Auth multi-facteur Premium/PME | Sécurité | Moyen |
| 15 | **Politique rétention** — Auto-suppression > 90j | RGPD | Faible |

### Monétisation & Croissance

| # | Proposition | Impact | Effort |
|---|-------------|--------|--------|
| 16 | **Freemium progressif** — Crédits avec gamification | Conversion +15% | Moyen |
| 17 | **Affiliation** — Parrainage crédits bonus | Acquisition | Moyen |
| 18 | **Product Hunt launch** — Page dédiée | Visibilité | Faible |
| 19 | **SEO landing pages** — Pages ciblées anti-spam | Trafic organique | Moyen |
| 20 | **API analytics** — Dashboard usage API B2B | Rétention B2B | Moyen |

---

## 📊 Résumé chiffré

| Catégorie | Nombre |
|-----------|--------|
| ✅ Terminé | ~70 items |
| 🔄 En cours | 4 items |
| 📋 À faire Q2 | ~25 items |
| 🔮 Rêve Q3-Q4+ | ~20 items |
| 💡 Améliorations proposées | 20 items |
| 📚 Fichiers documentation | 25 fichiers |

---

## 🗂️ Arborescence du projet (résumé)

```
├── src/
│   ├── pages/          # 10 pages (Home, Analyzer, Dashboard, PhoneChecker, SmsDashboard, Export, Settings, Aide, Auth, NotFound)
│   ├── components/     # 7 composants métier + 40+ composants UI shadcn
│   ├── hooks/          # 6 hooks (useEmailAnalyses, usePhoneChecks, useCommunityReports, useSpamAnalysis, useToast, useMobile)
│   ├── data/           # 4 fichiers JSON de patterns spam (29+ patterns)
│   ├── lib/            # credits.ts + utils.ts
│   └── integrations/   # Client Supabase + AuthProvider
├── supabase/functions/ # 5 Edge Functions (Deno)
├── supabase/migrations # SQL schéma + RLS
├── docs/               # 25 fichiers de documentation .md
│   └── V2/             # 6 fichiers documentation technique v2
├── scripts/            # Script extraction patterns
└── public/             # Assets statiques
```

---

**Dernière mise à jour :** 2026-04-17
**Prochaine revue :** Fin Q2 2026
