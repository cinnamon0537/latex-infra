# DevOps CI/CD Projekt – Gesamtdokumentation

## Projektziel
Aufbau einer vollständigen CI/CD-Pipeline für ein nicht‑triviales Softwareprojekt
auf eigener DevOps‑Infrastruktur.

---

## Softwareprojekt
**FastAPI LaTeX‑to‑PDF Compiler**
- Upload einer `.tex` Datei
- Sicherheitsprüfung
- Kompilierung via `pdflatex`
- Rückgabe einer PDF

Nicht trivial wegen:
- File Upload
- externer Toolchain
- Security Checks
- Unit Tests mit Mocking
- Dockerisierung
- CI/CD Automatisierung

---

## Infrastruktur
- Linux VM (AWS)
- GitLab CE
- GitLab Runner (Docker Executor)
- Docker + Docker‑in‑Docker
- Eigener DNS Server (BIND)

---

## Git Struktur
Branches:
- develop → Development Environment
- main/master → Production Environment

---

## CI/CD Anforderungen
- Lint Stage
- Test Stage
- Build Stage
- Deploy Stage
- Zwei Environments
- Docker Image
- Eigene Infrastruktur

---

## Pipeline Stages
1. Lint (ruff)
2. Test (pytest)
3. Build (Docker)
4. Deploy (Docker Registry)

---

## Lint Stage
- Static Code Analysis
- ruff check

---

## Test Stage
- pytest
- FastAPI TestClient
- Mocking für LaTeX

---

## Build Stage
- Docker Image Build
- docker:dind

---

## Deploy Stage
Deploy = Push des Images in Registry

Tags:
- develop → :develop
- main/master → :latest

---

## Docker Registry
- Docker Hub
- Auth via CI/CD Variables

Variables:
- DOCKERHUB_USERNAME
- DOCKERHUB_TOKEN

---

## Dockerfile
- Python 3.12
- TeX Live
- Requirements
- Uvicorn Start

---

## Typische Probleme & Lösungen
- Runner offline → falscher Token
- Docker DNS Fehler → BIND forwarders
- pdflatex fehlt → nur im Docker Image
- Tests schlagen fehl → Mocking

---

## Finaler Status
✅ CI/CD vollständig
✅ Zwei Environments
✅ Nicht‑triviales Projekt
✅ Eigene Infrastruktur

❌ Live Hosting nicht notwendig

---

## Ergebnis
Praktischer Teil abgeschlossen.
Übrig:
- Dokumentation
- Präsentation
