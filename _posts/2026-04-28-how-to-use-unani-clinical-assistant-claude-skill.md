---
layout: post
title: "How to Use Unani Clinical Assistant Claude Skill in Claude Code and Claude Desktop"
date: 2026-04-28 00:00:00 +0700
categories: [AI, Healthcare]
tags: [claude skills, claude code, claude desktop, unani medicine, clinical documentation, open source, ai for doctors, bums students]
comments: true
description: "A simple guide for doctors, BUMS students, and clinic teams to install, use, and manage the open-source Unani Clinical Assistant Claude Skill in Claude Code and Claude Desktop."
---

# How to Use Unani Clinical Assistant Claude Skill in Claude Code and Claude Desktop

Artificial intelligence is becoming useful for doctors, teachers, medical students, and clinic teams. But in healthcare, AI must be used carefully. It should support the doctor, not replace the doctor.

The **Unani Clinical Assistant Claude Skill** is an open-source Claude Skill made for Unani medicine education and clinical documentation. It can help BUMS students, Unani doctors, interns, teachers, and clinic teams create better drafts, organize patient notes, prepare study material, and improve documentation safety.

You can access the project here:

[Open the Unani Clinical Assistant GitHub repository](https://github.com/Faizee-Asad/unani-clinical-assistant)

This guide explains, in simple English, how to use the skill in **Claude Code** and **Claude Desktop**, how doctors can manage it safely, and what rules every user should follow.

---

## SEO keyword plan for this guide

This article is written for people searching for:

- Unani Clinical Assistant Claude Skill
- Claude Skill for doctors
- Claude Code medical skill
- Claude Desktop custom skill
- Unani medicine AI assistant
- AI clinical documentation for Unani doctors
- BUMS student AI study assistant
- OPD note assistant for Unani clinics
- Unani case taking template
- patient education handout for Unani doctors
- open source Claude Skill healthcare

The main search intent is practical: people want to know **what this skill does, how to install it, how to use it, and how to manage it safely**.

---

## What is a Claude Skill?

A Claude Skill is a small package of instructions, examples, templates, scripts, and reference files. It teaches Claude how to do a special type of work.

Instead of telling Claude the same rules again and again, you put those rules inside a Skill. Then Claude can use that Skill when the task is related.

For example, the Unani Clinical Assistant Skill tells Claude how to support tasks like:

- Unani case taking
- OPD note drafting
- follow-up note drafting
- patient education handouts
- red-flag and urgent referral checks
- BUMS viva and exam preparation
- Mizaj documentation
- Asbab-e-Sitta Zarooriya documentation
- Moalajat-style study notes
- Ilaj Bit Tadabeer documentation
- drug monograph drafting with verification fields
- research question and case-presentation support

The Skill is not a hospital system. It is not a replacement for a doctor. It is a safe documentation and education assistant.

---

## What is the Unani Clinical Assistant Skill?

The **Unani Clinical Assistant** is an open-source project for Unani medicine workflows. It includes a Claude Skill plus supporting files for safer clinical documentation.

The GitHub project includes:

- `SKILL.md` file for Claude
- safety policy
- clinical reference notes
- red-flag checklist
- Unani terminology reference
- patient education examples
- OPD documentation templates
- referral letter examples
- de-identified sample cases
- helper scripts
- tests and validation files
- contribution and security documents

The project is useful for doctors, students, and medical educators who want a structured way to use Claude for Unani medicine support.

It is especially helpful when you want Claude to follow a clear medical safety style instead of giving random or unsafe answers.

---

## Important medical safety warning

This Skill is for support only.

It should **not** be used for:

- final diagnosis
- autonomous prescribing
- medicine dose selection
- emergency triage
- replacing emergency care
- replacing a registered doctor
- replacing clinic protocol
- replacing local medical law
- making guaranteed cure claims
- storing real patient identifiers in public files

The safe way to use it is simple:

1. Give Claude de-identified information.
2. Ask it to create a draft.
3. Review the draft as a qualified doctor or under a qualified supervisor.
4. Verify every medicine, dose, contraindication, reference, and safety claim.
5. Keep the final clinical decision with the doctor.

AI can help organize thinking. It must not become the final clinical authority.

---

## Who should use this Skill?

This Skill can help many types of users.

### 1. Unani doctors

Doctors can use it to create structured drafts for:

- OPD notes
- follow-up notes
- patient education sheets
- referral letters
- missing history questions
- red-flag reminders
- lifestyle and Asbab-e-Sitta documentation

The doctor still makes the final decision.

### 2. BUMS students

Students can use it for:

- viva preparation
- case-presentation practice
- Moalajat topic summaries
- Unani terminology revision
- structured study notes
- comparing Unani framing with modern clinical correlation

Students should not use it to treat patients without supervision.

### 3. Interns

Interns can use it to prepare cleaner drafts for senior review. For example:

- “Make this case into an OPD note.”
- “List missing history questions.”
- “Prepare a referral draft.”
- “Explain red flags in simple words.”

### 4. Medical teachers

Teachers can use it to prepare:

- teaching cases
- viva questions
- class handouts
- structured clinical discussion prompts
- model case-presentation formats

### 5. Clinic teams

Clinic teams can use it to improve documentation quality, but they must use only de-identified patient data and follow clinic rules.

---

## How to access the project

Go to the GitHub repository:

[https://github.com/Faizee-Asad/unani-clinical-assistant](https://github.com/Faizee-Asad/unani-clinical-assistant)

You can download it as a ZIP or clone it with Git.

```bash
git clone https://github.com/Faizee-Asad/unani-clinical-assistant.git
cd unani-clinical-assistant
```

If you are a beginner, downloading the ZIP from GitHub is easier.

If you are a developer, cloning with Git is better because you can update it later.

---

## Project folder structure

After downloading the project, you will see folders like these:

```text
unani-clinical-assistant/
  skills/
    unani-clinical-assistant/
      SKILL.md
      safety-policy.md
      reference/
      templates/
      examples/
  docs/
  examples/
  scripts/
  tests/
  .github/
```

The most important folder is:

```text
skills/unani-clinical-assistant/
```

This is the Claude Skill folder.

The most important file is:

```text
skills/unani-clinical-assistant/SKILL.md
```

That file tells Claude how the Skill should behave.

---

## How to use the Skill in Claude Desktop or Claude web app

For normal Claude users, the easiest method is to upload the Skill as a ZIP file in Claude settings.

### Step 1: Build or get the Skill ZIP

If the GitHub release page has a ready-made file named something like this, download it:

```text
unani-clinical-assistant-skill.zip
```

If there is no ready-made Skill ZIP, build it from the repository:

```bash
python scripts/make_skill_zip.py
```

This should create:

```text
dist/unani-clinical-assistant-skill.zip
```

That is the file you upload to Claude.

### Step 2: Open Claude Skills settings

In Claude, go to:

```text
Settings or Customize > Skills
```

Make sure code execution and file creation are enabled if Claude asks for them.

### Step 3: Upload the Skill ZIP

Choose:

```text
Create Skill > Upload a skill
```

Then upload:

```text
dist/unani-clinical-assistant-skill.zip
```

After upload, the Skill should appear in your Skill list.

### Step 4: Turn the Skill on

Use the toggle button to enable the Skill.

If the Skill is turned off, Claude will not use it.

### Step 5: Test the Skill

Start a new chat and write:

```text
Use the Unani Clinical Assistant Skill.

Create a structured case-taking template for an adult patient with chronic constipation.
Include missing questions, red flags, and patient education.
Do not diagnose or prescribe.
```

Claude should respond with a structured and safety-aware format.

---

## How to use the Skill in Claude Code

Claude Code is useful if you want to work directly inside the project folder, edit files, build the Skill ZIP, run tests, and manage the project like a developer.

### Step 1: Clone the repository

```bash
git clone https://github.com/Faizee-Asad/unani-clinical-assistant.git
cd unani-clinical-assistant
```

### Step 2: Validate the Skill

Run:

```bash
python scripts/validate_skill.py skills/unani-clinical-assistant
```

This checks if the Skill folder has the required files.

### Step 3: Install developer dependencies

```bash
python -m pip install -e .[dev]
```

### Step 4: Run tests

```bash
python -m pytest
```

### Step 5: Build the upload ZIP

```bash
python scripts/make_skill_zip.py
```

This creates the upload file:

```text
dist/unani-clinical-assistant-skill.zip
```

### Step 6: Install the Skill for Claude Code

For a personal Skill available in all projects, copy it to:

```bash
mkdir -p ~/.claude/skills
cp -R skills/unani-clinical-assistant ~/.claude/skills/
```

Now you can use it in Claude Code with:

```text
/unani-clinical-assistant
```

Example prompt:

```text
/unani-clinical-assistant

Mode: OPD documentation
Role: BUMS intern
Case: Adult patient, age range 30–40, chronic constipation for several months, poor sleep, low appetite, sedentary lifestyle. No name, phone, address, or hospital ID included.
Need: structured OPD note, missing questions, red flags, patient education, and referral triggers.
Do not diagnose or prescribe.
```

---

## Personal Skill vs project Skill in Claude Code

There are two common ways to install a Skill in Claude Code.

### Option 1: Personal Skill

Use this path:

```text
~/.claude/skills/unani-clinical-assistant/SKILL.md
```

This makes the Skill available across your projects.

Best for:

- personal learning
- regular clinic documentation support
- daily use by one doctor or student

### Option 2: Project Skill

Use this path inside a project:

```text
.claude/skills/unani-clinical-assistant/SKILL.md
```

This makes the Skill available only for that project.

Best for:

- a clinic documentation repo
- a teaching project
- a research project
- a team that wants the same rules in one repository

For open-source work, project Skills are easy to share because they can be committed to GitHub.

---

## Best prompts for doctors

Good prompts are clear, safe, and specific. Do not simply write “treat this patient.”

Use a structured prompt like this:

```text
/unani-clinical-assistant

Mode: Case taking
Role: Registered Unani practitioner
Patient data: De-identified only
Chief concern: [write symptom and duration]
Need:
1. structured history questions
2. important negatives
3. red flags
4. Unani documentation headings
5. modern clinical correlation points to verify
6. patient education draft
Rules:
- Do not diagnose
- Do not prescribe
- Mark missing information
- Add referral triggers
```

This helps Claude give a safer and more useful answer.

---

## Example 1: OPD note draft

Prompt:

```text
/unani-clinical-assistant

Create an OPD note draft from this de-identified case.

Adult patient, age range 40–50, complains of knee pain for 8 months. Pain increases with walking and stairs. No fever reported. No recent trauma reported. Sleep is poor. Sedentary lifestyle. Current medicines unknown.

Need:
- structured OPD note
- missing questions
- examination points to record
- red flags
- patient education in simple English
- referral triggers

Do not diagnose or prescribe.
```

Possible output sections:

- Safety note
- Chief complaint
- History of present illness
- Relevant negatives
- Past history
- Drug history
- Allergy history
- Lifestyle and Asbab-e-Sitta points
- Unani documentation fields
- Examination points to record
- Red flags
- Missing information
- Patient education draft
- Clinician verification checklist

This saves time but still keeps review with the doctor.

---

## Example 2: Patient education handout

Prompt:

```text
/unani-clinical-assistant

Prepare a simple patient education handout for an adult patient with chronic constipation.
Use easy language.
Include diet, hydration, sleep, activity, warning signs, and when to seek medical care.
Do not include medicine names or doses.
```

This can help doctors explain common lifestyle advice in a patient-friendly way.

Always review the handout before giving it to a patient.

---

## Example 3: BUMS viva preparation

Prompt:

```text
/unani-clinical-assistant

I am a BUMS student.
Prepare viva notes for Zeequn Nafas.
Include:
- short definition
- Unani concepts
- important terms
- modern correlation
- case-presentation outline
- likely viva questions
- common mistakes to avoid

Keep it simple and exam-friendly.
```

This is useful for study and teaching. It should not be used as clinical authority.

---

## Example 4: Referral letter draft

Prompt:

```text
/unani-clinical-assistant

Draft a referral letter from this de-identified case.

Adult patient, age range 50–60, repeated chest discomfort with sweating and breathlessness. Symptoms occur on exertion. No patient name or ID included.

Need:
- urgent referral wording
- short clinical summary
- information to send with patient
- red flags
- do not diagnose
- do not delay emergency care
```

For serious symptoms, the Skill should use conservative language and advise urgent medical assessment.

---

## Example 5: Research support

Prompt:

```text
/unani-clinical-assistant

Help me create a research question for a Unani medicine observational study.
Topic: lifestyle factors and chronic constipation in adult OPD patients.
Need:
- research title ideas
- PICO or PECO format
- inclusion and exclusion ideas
- data collection headings
- ethics and privacy points
```

This can help students and researchers plan better, but the final protocol should be reviewed by supervisors and ethics committees.

---

## How doctors should manage the Skill in a clinic

Using an AI Skill in healthcare needs a simple management process.

### 1. Make a clinic policy

Write a short policy that says:

- who may use the Skill
- what tasks are allowed
- what tasks are not allowed
- what data can be entered
- who reviews the output
- where final notes are stored
- how errors are reported

### 2. Use de-identified data only

Do not put real patient identifiers into Claude or GitHub examples.

Remove:

- patient name
- phone number
- email
- address
- hospital ID
- government ID
- exact date of birth
- photo or scan with identity
- names of relatives
- exact village or workplace if it can identify the patient

Use safer wording like:

```text
Adult patient, age range 30–40, chronic constipation for several months.
```

### 3. Keep doctor review mandatory

Every output should be a draft.

Use labels like:

```text
Draft for clinician review only.
```

Do not copy AI output directly into final clinical records without checking it.

### 4. Verify medicines and references

For any drug, regimenal therapy, contraindication, classical reference, or modern evidence point, verify from trusted sources.

Do not allow the AI to invent references.

### 5. Keep an error log

If the Skill gives a wrong, unsafe, or confusing answer, record:

- date
- prompt used
- output problem
- correction
- whether the Skill file needs improvement

This makes the Skill better over time.

### 6. Update the Skill carefully

When you update the Skill:

1. Edit files in the repository.
2. Run validation.
3. Run tests.
4. Build a new Skill ZIP.
5. Upload the new ZIP.
6. Test with sample prompts.
7. Share the update notes with users.

---

## How to manage updates on GitHub

GitHub is useful because it keeps the project organized and open source.

A good open-source workflow is:

```bash
git pull
python scripts/validate_skill.py skills/unani-clinical-assistant
python scripts/check_no_phi.py examples skills docs
python scripts/make_skill_zip.py
python -m pytest
git status
```

Before publishing, check:

- no real patient data is committed
- examples are synthetic or de-identified
- safety wording is clear
- no dose or treatment is presented as final advice
- README is updated
- release notes are added
- checksum is added if you publish a ZIP file

If you create a release ZIP, add a checksum in `CHECKSUMS.txt`.

Example:

```text
5370893db26eedb148d61657cfc64b68ca31b5843889f6a277b85b025f2c0c77  unani-clinical-assistant-open-source-github.zip
```

This helps users verify that the file did not change.

---

## How to share the Skill with others

There are three common ways to share it.

### 1. Share the GitHub repository

Send this link:

[Unani Clinical Assistant on GitHub](https://github.com/Faizee-Asad/unani-clinical-assistant)

This is best for developers, contributors, teachers, and open-source users.

### 2. Share a GitHub Release

Create a release and attach:

```text
unani-clinical-assistant-skill.zip
```

This is best for normal users who only want to upload the Skill to Claude.

### 3. Share inside an organization

If your clinic, college, or team uses Claude Team or Enterprise, an owner may manage Skills for the organization.

This is best when many users need the same version.

---

## Troubleshooting

### Problem: Claude is not using the Skill

Try this:

- Check that the Skill is enabled.
- Start a new chat.
- Use the direct slash command: `/unani-clinical-assistant`
- Write clearly that you want Claude to use the Skill.
- Check that the Skill name and description are correct.

### Problem: Upload failed

Common causes:

- ZIP file has the wrong folder structure.
- `SKILL.md` is missing.
- The Skill folder name does not match the Skill name.
- ZIP file is too large.
- The Skill name has invalid characters.

Build the ZIP again:

```bash
python scripts/make_skill_zip.py
```

Then upload the ZIP from:

```text
dist/unani-clinical-assistant-skill.zip
```

### Problem: The answer is too general

Give more structure.

Bad prompt:

```text
Make Unani note.
```

Better prompt:

```text
/unani-clinical-assistant

Mode: OPD documentation
Role: Registered Unani practitioner
Need: structured note, missing questions, red flags, patient education, and referral triggers.
Rules: Do not diagnose or prescribe.
Case: [de-identified case details]
```

### Problem: The answer sounds unsafe

Stop using that answer.

Then improve the prompt:

```text
Add safety boundaries. Do not diagnose. Do not prescribe. Mark missing information. Add urgent referral triggers. Keep this as a draft for clinician review.
```

If the problem is in the Skill itself, open a GitHub issue or submit a pull request.

---

## Best practices for safe clinical use

Use these rules every time:

- Use de-identified data.
- Do not enter private patient identifiers.
- Ask for drafts, not final decisions.
- Ask for missing information.
- Ask for red flags.
- Ask for referral triggers.
- Keep doctor review mandatory.
- Verify medicines, dosages, and references.
- Use patient-friendly language for handouts.
- Do not use AI for emergency management.
- Do not publish real patient cases online.

A simple safe phrase is:

```text
This is a draft for clinician review. Do not diagnose or prescribe. Highlight missing information and urgent referral triggers.
```

---

## Why this Skill is useful for Unani medicine

Unani medicine has many detailed documentation areas, including Mizaj, Akhlat, Asbab-e-Sitta Zarooriya, Moalajat, and Ilaj Bit Tadabeer. A general AI chat may not organize these headings well unless you explain them every time.

The Unani Clinical Assistant Skill gives Claude a repeatable structure. That makes the output more consistent.

It can help users:

- save time
- write cleaner notes
- remember missing questions
- explain lifestyle advice clearly
- prepare teaching material
- practice case presentations
- improve patient safety documentation
- avoid unsafe AI claims

This is why an open-source Claude Skill is a good format for doctors and educators. Everyone can read the instructions, suggest improvements, and report problems.

---

## Frequently asked questions

### Is this Skill a medical device?

No. It is an open-source documentation and education assistant. It does not diagnose, prescribe, or replace a registered practitioner.

### Can I use it with real patient data?

Use de-identified data only. Do not enter names, phone numbers, addresses, hospital IDs, exact birth dates, or identifying images.

### Can students use it?

Yes, students can use it for learning, viva preparation, and case-presentation practice. They should not use it to treat patients without qualified supervision.

### Can doctors use it in OPD?

Yes, doctors can use it to draft notes, patient education, and checklists. The doctor must review and approve the final output.

### Does it prescribe Unani medicine?

No. It should not provide final prescriptions or patient-specific dosing. It can help organize documentation and list verification fields for qualified review.

### Can I edit the Skill?

Yes. It is open source. You can fork the repository, edit the files, test changes, and submit improvements.

### Where do I report problems?

Use the GitHub Issues section in the repository:

[Report an issue on GitHub](https://github.com/Faizee-Asad/unani-clinical-assistant/issues)

---

## Final thoughts

The **Unani Clinical Assistant Claude Skill** is a practical open-source tool for Unani doctors, BUMS students, medical teachers, and clinic teams. It helps turn Claude into a safer and more structured assistant for Unani medicine documentation and learning.

Use it for drafts, checklists, education, and organization. Do not use it as a replacement for clinical judgment.

If you want to try it, start here:

[Use the Unani Clinical Assistant Skill from GitHub](https://github.com/Faizee-Asad/unani-clinical-assistant)

Read the safety files, upload the Skill, test it with de-identified examples, and improve it with the open-source community.

---

## Helpful official resources

- [Claude Skills help guide](https://support.claude.com/en/articles/12512180-use-skills-in-claude)
- [Claude Code Skills documentation](https://code.claude.com/docs/en/skills)
- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code Desktop documentation](https://code.claude.com/docs/en/desktop)
- [Anthropic Skills GitHub repository](https://github.com/anthropics/skills)
