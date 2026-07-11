<h1 align="center">StatScreen</h1>

<p align="center"><em>A transparent, statistics-driven resume screener — you choose the signals, the model does the ranking.</em></p>

<p align="center">
  <img src="https://img.shields.io/badge/React-18-1ba14a?logo=react&logoColor=white" alt="React 18" />
  <img src="https://img.shields.io/badge/Vite-5-103a82?logo=vite&logoColor=white" alt="Vite 5" />
  <img src="https://img.shields.io/badge/Node-%E2%89%A518-1ba14a?logo=node.js&logoColor=white" alt="Node >=18" />
  <img src="https://img.shields.io/badge/Turso-libSQL-103a82?logo=sqlite&logoColor=white" alt="Turso libSQL" />
  <img src="https://img.shields.io/badge/model-logistic%20regression-1ba14a" alt="Logistic regression" />
  <img src="https://img.shields.io/badge/fairness-stratified%20SMOTE-103a82" alt="Stratified SMOTE" />
  <img src="https://img.shields.io/badge/parsing-in--browser-1ba14a" alt="In-browser parsing" />
</p>

<p align="center">
  <a href="https://statscreen.lovable.app"><strong>Live demo →</strong></a>
</p>

---

## Table of contents

- [Overview](#overview)
- [How it works — the method](#how-it-works--the-method)
- [Fairness: stratified SMOTE](#fairness-stratified-smote)
- [Features](#features)
- [The workflow](#the-workflow)
- [Getting started](#getting-started)
- [Deployment](#deployment)
- [Notes & limitations](#notes--limitations)
- [Credits](#credits)

---

## Overview

**StatScreen** turns a pile of resumes into a ranked, explainable shortlist for a role — without pretending to be a black box. A recruiter describes the job, chooses the signals that actually matter (papers cleared, technical skills, qualification, location, and so on), and the tool learns how much each signal should count from past hiring outcomes. Every candidate then gets a **probability of being a good hire**, and the list is sorted by it.

The guiding idea, drawn from the underlying research, is **recruiter control with statistical rigour**: the human decides *what* to measure; the model handles *how much each thing weighs* and *how to combine them consistently* — and flags where historical bias may be creeping in.

Try the live prototype at **[statscreen.lovable.app](https://statscreen.lovable.app)**.

---

## How it works — the method

At its core StatScreen is a **logistic regression** classifier. Each candidate is described by a set of predictor values `X1, X2, … Xn`, and the model estimates the probability of a positive outcome as:
