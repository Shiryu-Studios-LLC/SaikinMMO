# ARCHITECTURE DECISIONS — SaikinMMO

This document records authoritative architectural decisions for SaikinMMO.

Once a decision is recorded here, it is considered **settled** unless explicitly revised.
All agents and contributors must respect these decisions.

---

## Decision Log

### AD-001 — Server Authoritative Model
**Status:** Accepted  
**Date:** 2025-XX-XX  

The game uses a fully server-authoritative model.

- Clients are never trusted for game-critical state
- The server validates all actions
- Clients may predict locally but never decide outcomes

**Rationale:**  
Prevents cheating and simplifies long-term MMO scalability.

---

### AD-002 — Zone / Instance-Based World
**Status:** Accepted  
**Date:** 2025-XX-XX  

The world is divided into discrete zones or instances.

- No seamless single-world simulation
- Players transition between zones
- Each zone has its own authoritative simulation

**Rationale:**  
This is feasible for a solo developer and avoids distributed simulation complexity.

---

### AD-003 — Unreal Engine Client Responsibilities
**Status:** Accepted  
**Date:** 2025-XX-XX  

The Unreal Engine client is responsible for:
- Input collection
- Local prediction
- Rendering and presentation
- Sending requests to the server

The client is NOT responsible for:
- Combat resolution
- Inventory truth
- Persistence
- Security decisions

**Rationale:**  
Keeps authority centralized and reduces exploit surface.

---

### AD-004 — Backend as Source of Truth
**Status:** Accepted  
**Date:** 2025-XX-XX  

All persistent game state is owned by the backend.

This includes:
- Characters
- Inventory
- Progression
- Social state

The client holds only transient, cached representat

---

### AD-006 — Git Submodule Update Policy
**Status:** Accepted  
**Date:** 2025-XX-XX  

This repository uses Git submodules for major components (e.g., Backend, SaikinStory).

**Rules:**

- Each submodule is an independent Git repository.
- Changes MUST be committed and pushed inside the submodule repository first.
- The parent repository tracks submodules by commit pointer only.
- After pushing submodule changes that should be consumed by the project, the parent repository MUST:
  - Stage the submodule directory
  - Commit the updated submodule pointer
  - Push the parent repository

**Order of operations (mandatory):**
1. Commit and push changes in the submodule repository
2. Commit the updated submodule pointer in the parent repository
3. Push the parent repository

**Prohibited behaviors:**
- Relying on uncommitted submodule changes
- Assuming the parent repository updates submodule pointers automatically
- Pushing the parent repository without pushing submodules first
- Using force-push on submodules or parent repository except in explicitly documented recovery scenarios

**Rationale:**  
This prevents silent version drift, broken clones, inconsistent builds, and deployment errors.  
Explicit submodule pointer updates ensure reproducibility and long-term project stability.
