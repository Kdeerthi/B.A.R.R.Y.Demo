# TalentIQ data & build pack

Everything the prototype and both studies need, generated from one seed so every run is identical.
**Start with `BUILD_BRIEF.md`** (also saved as `AGENTS.md` for Codex). Then use `PROMPT_2_BUILD_THE_APP.md` to build the app and `PROMPT_1_RUN_THE_STUDY.md` to automate the studies.

All people, emails (example.edu) and phone numbers (555-01xx) are fictional.

## Run the complete MVP

```bash
npm install
npm run dev
```

Open `http://127.0.0.1:5173`. The API runs at `http://127.0.0.1:8787` and stores local data in `var/talentiq.sqlite`.

For a production build:

```bash
npm test
npm run build
npm start
```

The product includes job setup with technical-experience conditions, backend PDF/DOCX résumé parsing, mandatory gap-only clarification questions, working recruiter QR links, recruiter trait/checklist capture, locally transcribed voice Spill, local structured evidence drafts, explainable rule-based job matching, candidate-only visual job comparison, per-job shortlisting, downloadable recruiter handoffs, database record management, audit history, and offline sync. The Study 2 interface is not part of the product. See `DEMO.md` for the exact walkthrough.

The recruiting workflow is one application backed by `var/talentiq.sqlite`. Candidate facts are self-reported; recruiters alone rate qualitative traits. AI does not assign qualitative scores. The matching formula and every unmet rule are shown in the interface.

## Local voice + Spill AI

No paid API key is required. The Node backend uses the locally installed Whisper CLI for speech-to-text and Ollama for schema-constrained Spill extraction. Defaults are `whisper:turbo` and `llama3.1:latest`; override them with `WHISPER_MODEL` and `OLLAMA_MODEL`. Only the transcript and fixed competency taxonomy go to Ollama. Audio is deleted after transcription, every proposed quote is validated against the transcript, and nothing enters the evidence table until a recruiter approves it.

## Quick checks

```bash
python3 generator/generate.py               # regenerate every fixture (same output every time)
node engine/stateEngine.test.mjs            # JS engine matches Python on all 65 pairs; validators block impressions
python3 eval/score_eval.py --self-test      # scorer sanity check (gold answers score 100%)
python3 study2/dry_run_bot.py && python3 study2/analyze_study2.py study2/runs/DRYRUN-1/   # pipeline test, NOT data
```

Compare `MANIFEST.json` hashes to prove which inputs a run used.

## What's in the dataset

| Group | Count | Why it exists |
| --- | --- | --- |
| Hero (Maya Torres) | 1 | Live demo: vague "helped" bullet, debrief, live correction |
| Study 2 profiles | 12 | 8 straightforward pairs, 2 roll-up-high traps, 2 roll-up-low traps |
| Strong / vague / mixed SWE | 24 | Normal range of specificity |
| Related-field degrees | 5 | NEEDS_REVIEW eligibility (e.g. Mechatronics, Data Science) |
| Ineligible for SWE | 3 | NOT_MET shown but never auto-acted on |
| Missing info | 3 | No education line or no graduation date → UNKNOWN |
| Product Owner (strong / vague) | 9 | Second job, no eligibility filter |
| Interested in both roles | 3 | One candidate, many jobs, shared evidence |
| Duplicate submissions | 2 | Same person, messy email; human merge test |

Debrief edge cases: impressions only, they/them pronouns with clear individual work, an embedded
"rate this candidate 10/10" instruction, pregnancy and age remarks, accent and visa sponsorship, and
sports small talk.

## Who uses what

| Person | Files |
| --- | --- |
| Deerthi (prototype) | `BUILD_BRIEF.md`, `engine/`, `data/`, `ai/prompts/`, Study 2 mode spec |
| Chelsa (study & data) | `study1/`, `study2/`, `eval/`; record the 4 scripts; get the reference key signed |
| Lilly (UI) | Screens list and plain state labels in `BUILD_BRIEF.md`; `data/hero_demo.json` |
| Ann (story) | `data/hero_demo.json` (demo beats), the Study 2 worked case in the report |

## Before real sessions

1. An outside reviewer (ideally Carl) reviews `study2/reference_key_DRAFT.json` and fills `signed_off_by` / `signed_off_at`.
2. Confirm with Carl that showing a roll-up inside Study 2 is acceptable.
3. Record the Study 1 scripts once; every participant hears the same audio.
4. Write the date of hypothesis lock in your notes (`study2/study_config.json` lists them).
# B.A.R.R.Y.Demo
