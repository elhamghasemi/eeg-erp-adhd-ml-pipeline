# eeg-erp-adhd-ml-pipeline

End-to-end EEG/ERP pipeline for ADHD detection: band decomposition, feature engineering, EDA, and model comparison.

> **Goal:** Investigate which signal features best separate **ADHD vs Normal** and which ML models perform best.

---

## Dataset (summary, not redistributed)

- **Participants:** 60 children (ages **4–15**); **30 ADHD** and **30 controls** (DSM-IV). ADHD participants on methylphenidate withheld medication **24 h** prior to testing. All had normal vision/hearing, no other neurological disorders, IQ **> 80**.
- **Paradigm:** Oddball (auditory + visual), **200 stimuli** (~**80%** non-target, **20%** target). Task: press a button on rare stimuli.
- **Acquisition:** Midline **Fz, Cz, Pz** (10–20), **640 Hz** sampling, NicoletOne. Each lead treated independently → **6 trials/subject**.
- **Data sharing:** Original data is **not redistributed**. See **Data access** below to reproduce locally.

---

## Repository structure

