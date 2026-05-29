# Public Release Framework for Technical Learning Artifacts

This document defines the content boundary between private study materials and public-facing documentation in this repository. It ensures that all published content is original, professional, and free of restricted material.

---

## Guiding Principle

This repository publishes only original, synthesized artifacts. Raw course content, transcripts, quiz answers, and copied instructional text are never committed. Every document represents my own understanding, expressed in my own words, and formatted for professional readability.

---

## 1. Safe to Publish as Original Notes

Content that has been fully transformed into professional, role-aligned documentation:

- **Technical summaries** — High-level overviews of hardware, networking, cloud, and security concepts written in my own words and connected to OTS responsibilities
- **Custom runbooks and SOPs** — Original step-by-step troubleshooting guides derived from course concepts but structured for IT support engineering use
- **Role-mapping documents** — Tables and guides that connect IT fundamentals to specific Amazon OTS responsibilities
- **Glossaries** — Original definitions with plain-English explanations and workplace-relevant examples
- **Architecture descriptions** — Written explanations of infrastructure designs (VPC layouts, subnet plans, network topologies)

---

## 2. Safe to Publish as Personal Reflections

Content focused on learning progress and professional growth:

- **Weekly learning logs** — Summaries of what was studied, key takeaways, areas needing more practice, and next steps
- **Learning roadmaps** — Career path updates showing progression through study phases
- **Career rationale** — Professional explanations of why specific certifications or specializations were chosen based on role requirements
- **Skill gap analysis** — Honest assessments of what I know, what I am learning, and what I still need to develop

---

## 3. Safe to Publish as Lab Screenshots

Visual evidence of hands-on technical work:

- **Infrastructure proof** — Screenshots of custom VPC setups, route tables, security group configurations in the AWS console
- **Terminal outputs** — Captured results from successful CLI commands (ping, ipconfig, nslookup, ls, cat) demonstrating practical use
- **Architecture diagrams** — Professional drawings (from draw.io or similar tools) showing network topologies, VPC designs, or traffic flows
- **Certificate images** — Course completion certificates (with personal details reviewed before posting)

---

## 4. Do Not Publish Directly

Content that must remain in private notes (Obsidian, NotebookLM, or local files):

- **Raw course transcripts** — Verbatim text from video lectures, captions, or slide decks
- **Quiz and assessment content** — Specific questions, answer choices, or correct answers from any course
- **Lecture slides or copied instructional text** — Direct reproductions of course materials
- **Private keys and credentials** — `.pem` files, SSH private keys, API keys, passwords, tokens
- **AWS account identifiers** — Account IDs, access key IDs, secret access keys
- **Personal identity data** — Phone numbers, credit card information, physical addresses, or other PII used during account setup
- **Internal Amazon information** — Any proprietary processes, tools, or systems not publicly documented

---

## 5. Needs Rewriting Into My Own Words First

Content that is valuable but requires transformation before publishing:

- **Course analogies** — Instructional metaphors should be replaced with original explanations or professional definitions
- **Technical definitions** — Terms defined in course materials should be rewritten through my own understanding and connected to OTS context
- **Tutorial steps** — Lab procedures should be converted into original SOPs or runbooks rather than reproduced as step-by-step copies
- **Concept explanations** — Any explanation that closely mirrors source wording needs to be fully rewritten with original structure and examples

---

## .gitignore Enforcement

The repository's `.gitignore` file prevents accidental commits of:

```
_source-inbox/
private-notes/
*.pem
*.key
.env
.env.*
credentials*
secrets*
```

---

## Review Checklist Before Committing

Before any commit, verify:

- [ ] No raw transcript text or copied course content
- [ ] No quiz questions or assessment answers
- [ ] No private keys, tokens, or credentials
- [ ] No AWS account IDs or secret keys
- [ ] No personal identity information
- [ ] All technical content is written in my own words
- [ ] Content is framed as learning-in-progress, not production experience
- [ ] Writing is professional and recruiter-readable
