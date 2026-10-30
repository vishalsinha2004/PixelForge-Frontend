# PIXELFORGE AI — PROJECT MASTER PLAN

## Architecture & Separation Guardrails
1. **Repository A (`PixelForge-AI-Frontend`)**: React + TypeScript + Vite + Tailwind CSS + Zustand + Monaco Editor. Deployed on Netlify.
2. **Repository B (`PixelForge-AI-Backend`)**: Python + Django + DRF + Supabase (PostgreSQL, Auth, Storage) + `google-genai` SDK + Pillow + Playwright. Deployed on Render.
3. **Generated Output Constraint**: Every generated website MUST be pure HTML5, CSS3, and Vanilla JavaScript (`index.html`, `style.css`, `script.js`, `project-manifest.json`). Never generate React/Vue/Next.js for user websites.

---

## Numbered Implementation Roadmap

### Phase 0 — Existing Project Audit
| Step ID | Objective | Dependencies | Acceptance Criteria | Status |
| :--- | :--- | :--- | :--- | :--- |
| **0.1** | Inspect `PixelForge-AI-Frontend` structure, configs, and existing components | None | Full directory tree, `package.json`, `vite.config.ts`, and routes documented | BLOCKED (Awaiting repo tree) |
| **0.2** | Inspect `PixelForge-AI-Backend` structure, Django apps, models, and endpoints | None | Full directory tree, `requirements.txt`, `settings.py`, and installed apps documented | BLOCKED (Awaiting repo tree) |
| **0.3** | Run baseline frontend (`vitest`) and backend (`pytest`) test suites | 0.1, 0.2 | Exact test command outputs recorded; broken/incomplete features cataloged | NOT STARTED |
| **0.4** | Commit canonical `PROJECT_MASTER_PLAN.md` and `PROJECT_STATUS.md` to both repos | 0.1, 0.2, 0.3 | Files exist in both repositories; first unfinished implementation step locked | IN PROGRESS |

### Phase 1 — Foundation
| Step ID | Objective | Dependencies | Acceptance Criteria | Status |
| :--- | :--- | :--- | :--- | :--- |
| **1.1** | Configure Django settings, `/api/v1/health/` check, CORS, and `.env.example` | 0.4 | `GET /api/v1/health/` returns 200 OK with DB status; CORS locked to frontend origin; `pytest` passes | NOT STARTED |
| **1.2** | Configure Supabase PostgreSQL connection & core models (`accounts`, `projects`, `usage`) | 1.1 | Models for `UserProfile`, `Project`, `ProjectVersion`, `ProjectFile`, `GenerationJob` migrated & tested | NOT STARTED |
| **1.3** | Implement Supabase JWT Authentication middleware & DRF permission classes | 1.2 | Unauthenticated requests return 401; users cannot access other users' projects (403/404 verified by tests) | NOT STARTED |
| **1.4** | Implement Project & File CRUD APIs (`/api/v1/projects/`) with Supabase Storage backing | 1.3 | Create, list, retrieve, delete projects; persist source files outside ephemeral Render disk | NOT STARTED |
| **1.5** | Setup Frontend API client (Axios interceptors), Auth store (Zustand), and Protected Routes | 1.3, 1.4 | Frontend login/signup via Supabase Auth attaches Bearer token; `/dashboard` guarded | NOT STARTED |
| **1.6** | Build Main Dashboard UI (Project listing, search/filter, creation modal, status badges) | 1.5 | Real projects fetched from `/api/v1/projects/`; loading, empty, and error states verified via Vitest | NOT STARTED |

### Phase 2 — Screenshot to Website
| Step ID | Objective | Dependencies | Acceptance Criteria | Status |
| :--- | :--- | :--- | :--- | :--- |
| **2.1** | Backend screenshot upload & Pillow validation endpoint (`/api/v1/generation/screenshot/upload/`) | 1.4 | Validates MIME type (PNG/JPEG/WebP), file size, dimensions; stores in Supabase Storage | NOT STARTED |
| **2.2** | Frontend Screenshot Upload workspace (`react-dropzone`, dimension checks, settings panel) | 1.6, 2.1 | Drag-and-drop preview, client-side validation, uploads to backend and receives asset ID | NOT STARTED |
| **2.3** | Gemini Vision Analysis service (`google-genai`) for layout, color, typography, and DOM extraction | 2.1 | Produces structured JSON layout specification from uploaded screenshot; handles timeouts/rate-limits | NOT STARTED |
| **2.4** | HTML5/CSS3/Vanilla JS code generator & syntax/structure validator | 2.3 | Outputs strict `index.html`, `style.css`, `script.js`, `project-manifest.json`; blocks framework code | NOT STARTED |
| **2.5** | Isolated Sandbox Preview Service & separate origin preview serving | 2.4 | Renders generated HTML/CSS/JS with strict CSP, sandboxed iframe/worker, no host secrets | NOT STARTED |
| **2.6** | Playwright visual comparison & automated targeted correction loop | 2.5 | Captures preview screenshot at matching viewport, compares with original, runs bounded repair pass | NOT STARTED |

### Phase 3 — Description to Website
| Step ID | Objective | Dependencies | Acceptance Criteria | Status |
| :--- | :--- | :--- | :--- | :--- |
| **3.1** | Prompt-to-Specification AI service (`/api/v1/generation/description/`) | 2.4 | Converts natural-language prompt + optional style settings into structured JSON spec | NOT STARTED |
| **3.2** | Description-to-Website code generation, validation, and sandbox preview integration | 3.1, 2.5 | Generates multi-section vanilla HTML/CSS/JS website, persists version 1, runs Playwright smoke test | NOT STARTED |
| **3.3** | Frontend Description-to-Website workspace & real-time job polling UI | 1.6, 3.2 | Form with optional category/palette/typography controls, live job progress indicator, transitions to editor | NOT STARTED |

### Phase 4 — Post-Generation Website Editing
| Step ID | Objective | Dependencies | Acceptance Criteria | Status |
| :--- | :--- | :--- | :--- | :--- |
| **4.1** | Frontend Editing Studio layout (File Explorer, Monaco Editor, responsive preview toggles, manual save) | 2.5, 3.3 | Edit `index.html`/`style.css`/`script.js` in Monaco, save new `ProjectVersion`, switch Desktop/Tablet/Mobile | NOT STARTED |
| **4.2** | Backend AI Targeted Patch Engine (`/api/v1/editing/patch/`) | 3.2 | Inspects current files, applies natural-language modification as targeted diff without wiping unrelated code | NOT STARTED |
| **4.3** | Frontend AI Modification Panel & Monaco Diff Review Modal (Accept / Reject / Retry) | 4.1, 4.2 | Shows side-by-side diff before committing; rolls back cleanly on rejection or validation failure | NOT STARTED |
| **4.4** | Version History, Rollback API (`/api/v1/projects/{id}/versions/{v_id}/restore/`), & ZIP Export | 4.3 | Full version tree with parent IDs, one-click restore creates new checkpoint, downloadable standalone `.zip` | NOT STARTED |

### Phase 5 — Initial Release Quality & Security Hardening
| Step ID | Objective | Dependencies | Acceptance Criteria | Status |
| :--- | :--- | :--- | :--- | :--- |
| **5.1** | Quota & rate-limiting enforcement (`usage` app) + cross-tenant security audit | 4.4 | Per-user daily generation/edit caps enforced; IDOR, XSS, CSRF, SSRF, and path traversal tests pass | NOT STARTED |
| **5.2** | Complete documentation (`ARCHITECTURE.md`, `DECISIONS.md`, `TESTING_GUIDE.md`, `DEPLOYMENT_GUIDE.md`) | 5.1 | All operational guides synchronized with real code and verified commands | NOT STARTED |

### Phases 6–9 — Post-MVP & Production Release
| Step ID | Objective | Dependencies | Acceptance Criteria | Status |
| :--- | :--- | :--- | :--- | :--- |
| **6.1** | Multi-Page Website Generation (`pages/*.html`, shared CSS, internal link validator) | 5.2 | Generates connected multi-page HTML sites with verified navigation links | NOT STARTED |
| **7.1** | Wireframe/Sketch-to-Website pipeline | 6.1 | Converts low-fidelity wireframe uploads into styled, responsive HTML/CSS/JS websites | NOT STARTED |
| **8.1** | Safe ZIP Import & Redesign (Zip-slip path validation, asset check, targeted redesign) | 7.1 | Rejects traversal archives; imports valid HTML/CSS/JS zips into versioned projects | NOT STARTED |
| **9.1** | Production Deployment verification (Netlify + Render + Supabase RLS + Health monitoring) | 5.2 | Live production URLs verified end-to-end with passing smoke tests | NOT STARTED |