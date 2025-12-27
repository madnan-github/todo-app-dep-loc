---
id: 001
title: TaskFlow Constitution Creation
stage: constitution
date: 2025-12-27
surface: agent
model: claude-opus-4-5-20251101
feature: none
branch: master
user: ruser
command: direct-prompt
labels: ["constitution", "free-tier", "project-setup", "spec-driven"]
links:
  spec: null
  ticket: null
  adr: null
  pr: null
files:
  - .specify/memory/constitution.md
tests:
  - N/A (documentation artifact)
---

## Prompt

read throughly my complete project file i.e. project-detail.md, i want to write a complete constitution based on my project, keep remember one thing that in this project use all free API_KEY supported portals, suggest me how to do it.

## Response snapshot

Created comprehensive constitution for TaskFlow project at `.specify/memory/constitution.md` with:

**7 Core Principles:**
1. Spec-Driven Development (Non-Negotiable)
2. AI-First Development
3. Test-First (TDD Mandatory)
4. Free-Tier First
5. Progressive Architecture
6. Stateless & Cloud-Native Design
7. Simplicity & YAGNI

**Technology Stack Decisions (All Free-Tier):**
- **LLM**: Groq (completely free tier)
- **Database**: Neon PostgreSQL (0.5GB free)
- **Cloud K8s**: Oracle Cloud OKE (always free - 4 OCPUs, 24GB RAM)
- **Event Streaming**: Redpanda Cloud (free serverless) or Strimzi self-hosted
- **Frontend Hosting**: Vercel (100GB bandwidth free)
- **CI/CD**: GitHub Actions (2000 mins/mo free)

**Phase Requirements Documented:**
- Phase I: Console App (100 pts) - Python, UV, Rich, Pydantic
- Phase II: Full-Stack Web (150 pts) - Next.js, FastAPI, SQLModel, Better Auth
- Phase III: AI Chatbot (200 pts) - Groq, OpenAI Agents SDK, MCP
- Phase IV: Local K8s (250 pts) - Docker, Minikube, Helm
- Phase V: Cloud Deploy (300 pts) - Oracle OKE, Dapr, Redpanda

## Outcome

- Impact: High - Established foundational project principles and free-tier technology decisions
- Tests: N/A (documentation artifact)
- Files: 1 file created (.specify/memory/constitution.md)
- Next prompts: `/sp.specify` to create Phase I feature specification
- Reflection: User prioritized free-tier services; selected Groq over OpenAI for LLM, Oracle Cloud over Azure/GCP for always-free K8s

## Evaluation notes (flywheel)

- Failure modes observed: None
- Graders run and results (PASS/FAIL): PASS - All placeholders filled, principles actionable
- Prompt variant (if applicable): N/A
- Next experiment (smallest change to try): Begin Phase I specification with `/sp.specify`
