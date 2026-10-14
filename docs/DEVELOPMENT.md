# TrafficPulse-X Development Guide

This guide outlines setup, execution, testing, and repository development conventions for TrafficPulse-X.

---

## 1. Environment Requirements

- **Python**: Python 3.10+ (Tested on Python 3.14.7)
- **Node.js**: Node.js v18+ / v20+ (Tested with npm & Vite v8.3.3)

---

## 2. Installation & Setup

### Backend Setup
1. Create and activate a Python virtual environment:
   ```bash
   python -m venv .venv
   # Windows (PowerShell)
   .\.venv\Scripts\Activate.ps1
   # Linux/macOS
   source .venv/bin/activate
   ```
2. Install direct Python dependencies:
   ```bash
   python -m pip install -r backend/requirements.txt
   ```

### Frontend Setup
1. Install Node dependencies:
   ```bash
   npm install
   ```

---

## 3. Running TrafficPulse-X

### Starting the Backend API Server
Start the single FastAPI web application instance on port 8000:
```bash
# From workspace root
python -m uvicorn backend.main:app --reload --port 8000
```
API Documentation will be accessible at `http://localhost:8000/docs`.

### Starting the Frontend Development Server
Start the React Vite development server on port 5173:
```bash
npm run dev
```
The React User Interface will be accessible at `http://localhost:5173`.

---

## 4. Testing & Verification

### Running Backend Unit & Regression Tests
Run the full 343-test backend pytest suite:
```bash
python -m pytest backend -q
```
All **343 tests** must pass.

### Frontend Linting & Production Build
Verify ESLint and the Vite production build succeed:
```bash
npm run lint
npm run build
```

---

## 5. Branch Workflow & Pull Request Expectations

1. **Main Branch Protection**: The `main` branch represents production-ready, submission-frozen code.
2. **Feature Branching**: Development occurs on descriptive feature or fix branches (e.g., `feat/decision-metrics`, `fix/route-nav`).
3. **Pull Request Requirements**:
   - Clean lint (`npm run lint` with 0 errors).
   - Successful production bundle (`npm run build`).
   - 100% backend test pass (`pytest backend -v` passing all 300 tests).
   - Zero hardcoded secrets or environment dumps.
   - Preserved scientific provenance and model checkpoints.

---

## 6. Environment Variables & Secrets

- **Frontend**: Contains **zero** secrets. All frontend configuration connects to the backend API (`http://localhost:8000/api`).
- **Backend**:
  - `GEMINI_API_KEY` (Optional): Set in `.env` or system environment for the server-side Gemini AI assistant proxy (`backend/assistant.py`).
  - `.env` is ignored by `.gitignore` and must never be committed to source control.

---

## 7. Key Repository Conventions

1. **Single FastAPI Rule**: All API routes are declared in `backend/main.py` under `/api/*`. Do NOT spawn parallel backend servers.
2. **Frontend Gateway Rule**: All UI components fetch data through `src/services/api.js`. Do NOT write component-level `fetch` calls.
3. **No Environment Dumps (`pip freeze`)**: Keep `backend/requirements.txt` restricted to direct project runtime, ML, and test dependencies.
4. **Frozen Scientific Checkpoints**: Model weight files (`graph_lstm_best.pt`, `global_best.pt`) and evaluation summaries are frozen contracts. Do NOT retrain or overwrite them without explicit directives.
5. **Standardized Terminology**: Refer to dataset replay as *Historical Replay*, FL participants as *Regional FL Clients*, and Need Score as a *DERIVED* 9-factor metric.
