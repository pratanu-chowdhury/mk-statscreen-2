Overview
StatScreen turns a pile of resumes into a ranked, explainable shortlist for a role — without pretending to be a black box. A recruiter describes the job, chooses the signals that actually matter (papers cleared, technical skills, qualification, location, and so on), and the tool learns how much each signal should count from past hiring outcomes. Every candidate then gets a probability of being a good hire, and the list is sorted by it.

The guiding idea, drawn from the underlying research, is recruiter control with statistical rigour: the human decides what to measure; the model handles how much each thing weighs and how to combine them consistently — and flags where historical bias may be creeping in.

Built for MK Recruitments — Actuaries · CA · CFA · FRM · US CPA · Data Analytics Jobs.

How it works — the method
At its core StatScreen is a logistic regression classifier. Each candidate is described by a set of predictor values X1, X2, … Xn, and the model estimates the probability of a positive outcome as:

logit(p) = b0 + b1*X1 + b2*X2 + … + bn*Xn
p        = 1 / (1 + e^(-logit(p)))
Predictors are configurable. Each one is count, binary, or ordinal, with keyword rules that read the value straight out of a resume. Add, remove, or rename them per role.
Weights are learned from data. Given a table of past candidates and whether each was a good hire (Y = 1/0), the coefficients are fit by maximum likelihood using IRLS (iteratively reweighted least squares) with light ridge regularisation for stability.
…or set by hand. With no history, a recruiter can dial the weights directly; the app scores from those instead. Learned and manual weights can be mixed, and overridden ones are flagged.
Probability, not a verdict. Candidates are ranked by p; a movable shortlist cutoff decides who gets forwarded, so nobody is silently rejected by the machine.
Every score is explainable. Click a candidate to see each predictor's contribution to their log-odds — exactly why they landed where they did.
Fairness: stratified SMOTE
Historical hiring data often carries bias. If past hires skewed toward one location, a naive model learns "that location = good hire" and quietly penalises everyone else — even when they're stronger on merit.

Stratified SMOTE addresses this. It oversamples the under-represented group within the successful (Y = 1) candidates by interpolating new synthetic examples inside that stratum, so the model stops leaning on the proxy and re-weights toward genuine signal.

On the tool's bundled sample data the effect is concrete:

Biased model	After stratified SMOTE
Historical selection rate	22% out-of-region vs 67% in-region	rebalanced
Weight on location	2.59 (dominates)	1.90 (reduced)
Weight on qualification	~0	becomes a real positive signal
A strong out-of-region candidate	p = 0.43 -> held	p = 0.67 -> surfaced
Same person, same resume — the bias correction is what lets merit rise to the top.

Features
Configurable predictors — count / binary / ordinal, with editable keyword rules.
JD scanning — paste a job description and pull detected skills, locations, qualifications and exam terms straight into your predictors.
Learn or set weights — fit from past hires, or tune by hand; mix both.
In-browser resume parsing — drop in PDFs, a ZIP, or text; files are read on your device and never uploaded for scoring.
Explainable ranking — probability per candidate, movable shortlist cutoff, per-candidate contribution breakdown.
Bias control — stratified SMOTE toggle with before/after transparency.
Saved screenings — persist an entire setup (JD, predictors, data, weights, candidate pool) to a shared database, with automatic browser-storage fallback.
The workflow
Job description — paste or upload the JD; StatScreen highlights signal terms.
Predictors — choose and configure the signals to read from each resume.
Training data — past candidates scored on those signals, with the hire outcome; weights refit live.
Weights — review the learned coefficients, override any by hand.
Screen resumes — upload the candidate pool, get a ranked, explainable shortlist.
Getting started
npm install
npm run dev        # app on http://localhost:5173
Runs immediately with browser-based storage — no database required to try it.

Deployment
StatScreen is a Vite + React frontend plus a small Express API backed by Turso (libSQL/SQLite) for shared, persistent storage. The database token lives only on the server; the browser only ever talks to /api, and falls back to local storage when no API is present. The frontend can be hosted on any static/app platform and the API on any Node host; a render.yaml blueprint is included for a one-service deploy.

Notes & limitations
Decision support, not a decision-maker. Scores come from keyword extraction plus chosen weights; a location-style predictor can encode bias, so shortlists should always be reviewed by a person.
Extraction is keyword/regex-based, not full natural-language understanding.
Privacy — resumes are parsed in the browser and are not sent anywhere for scoring.
Credits
Method — after Pratanu Chowdhury, Statistical Applications in Resume Screening Automation: manual predictor selection, logistic regression, and stratified SMOTE for historical bias correction.
Built for — MK Recruitments · Actuaries | CA | CFA | FRM | US CPA | Data Analytics Jobs.
