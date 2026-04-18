---
id: index
title: API Reference
sidebar_label: Vue d'ensemble
sidebar_position: 1
description: Référence complète des 5 endpoints API de BlocMail & BlocNumEtSMS
---

# 🔌 API Reference

## Base URL

```
https://spams.seb205.ovh/functions/v1/
```

## Authentification

Toutes les requêtes nécessitent le header d'authentification Supabase :

```http
Authorization: Bearer <supabase-anon-key>
Content-Type: application/json
```

## Endpoints disponibles

| Endpoint | Méthode | Description |
|----------|---------|-------------|
| [`/analyze-email-headers`](./email-headers) | POST | Analyse SPF/DKIM/DMARC/IP depuis headers bruts |
| [`/analyze-spam`](./spam-analysis) | POST | Scoring 29+ patterns + score hybride |
| [`/arcep-check`](./arcep-check) | POST | Vérification numéro dans base ARCEP |
| [`/trigger-n8n-export`](./webhooks) | POST | Déclenchement webhook n8n |
| [`/trigger-make-scenario`](./webhooks) | POST | Déclenchement webhook Make.com |

## Système de crédits

Les endpoints `analyze-email-headers` et `arcep-check` consomment **1 crédit** par appel (quota : 5/jour, reset minuit UTC).

## Codes d'erreur communs

| Code HTTP | Signification |
|-----------|--------------|
| `200` | Succès |
| `400` | Données invalides ou champ manquant |
| `401` | Non authentifié |
| `429` | Quota de crédits dépassé |
| `500` | Erreur serveur interne |

## API publique REST v1 (roadmap Q2 2026)

L'API publique avec **Bearer token** et **rate limiting** est prévue pour Q2 2026. Elle exposera :

- `POST /api/v1/email/analyze`
- `POST /api/v1/phone/check`
- Documentation **OpenAPI 3.0 + Swagger UI**
- Plans : 100 req/min · 10 000/jour

Voir [Roadmap Q2 2026](../roadmap/q2-2026).
