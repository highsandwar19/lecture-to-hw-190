<div align="center">

# lecture-to-hw

<p>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-yellow.svg" alt="License: MIT"></a>
  <img src="https://img.shields.io/badge/Codex-Skill-7C3AED" alt="Codex Skill">
  <img src="https://img.shields.io/badge/Multi--Agent-Workflow-2563EB" alt="Multi-Agent Workflow">
  <img src="https://img.shields.io/badge/README-English-22C55E" alt="README English">
</p>

<p>
  <a href="README.md">中文</a> | <a href="README_EN.md">English</a>
</p>

<p><strong>A Codex skill that turns course lectures into submission-ready Markdown homework.</strong></p>

</div>

The saddest part of college is not always hard homework. Sometimes it is spending an entire evening on a low-stakes course assignment.

You gather files, locate the homework PDF, search through lecture slides, paste questions into an LLM, review the answer, try to understand a few obscure concepts, decide whether the answer is trustworthy, and then remove the unmistakable AI smell from the final text.

Repeat that loop enough times and what you gain may not be knowledge. It may just be prompt-engineering muscle memory, exhaustion, and the familiar guilt of wasting another night.

So I packaged my own long-running homework workflow into a Codex skill:

**lecture-to-hw**

Its goal is simple: **hand the full pipeline to an agent, from course materials to a deliverable Markdown solution, end to end, with as much robustness as possible.**

The boring loop becomes:

```text
lecture-to-hw, get to work! --> grab a coffee --> review and submit
```

## What It Does

`lecture-to-hw` searches the current course directory for:

- homework prompts: PDF, DOCX, Markdown, HTML, images, notebooks, and more;
- course materials: lecture slides, classroom demos, experiment code, datasets;
- previous submissions: for example `作业/hw*_solution/*.md`.

Then it connects the pieces:

```text
read prompt -> find lectures -> use taught methods -> decompose tasks -> solve/run code -> assemble Markdown -> TA-agent review -> deliver
```

It is not just a homework solver. It is a small workflow factory that understands messy course folders.

## Why lecture-to-hw

### 1. From Lecture to Finished Homework

Most LLM homework workflows start by reading the problem and immediately answering it.

`lecture-to-hw` first looks for the relevant lectures and classroom code, then tries to use the formulas, terminology, algorithms, and notation taught in class.

If classroom code can reproduce an experimental result, it uses that instead of inventing numbers. If the lecture contains the expected method, it avoids over-engineered outside techniques.

### 2. Multi-Format Homework Prompts

It supports common course-material formats:

- PDF
- DOCX / DOC
- Markdown / TXT
- HTML
- notebooks
- image-based prompts
- homework files inside archives

If formulas, tables, images, or layout are uncertain, it should flag the uncertainty and ask for confirmation instead of confidently writing nonsense.

### 3. Multi-Agent Parallelism

The main agent acts as a controller:

```text
read -> decompose -> dispatch -> verify -> assemble
```

When the homework can be cleanly split by problem, experiment module, or lecture topic, it can run up to 4 subagents by default. Subagents only handle bounded tasks, such as:

- drafting one problem;
- reproducing one experiment or code result;
- matching one lecture to one task;
- independently verifying a module.

For short or tightly coupled assignments, it falls back to single-agent mode. No parallelism for the sake of parallelism.

### 4. TA-Style Review Agent

After the Markdown is assembled, a critic agent can review it from a teacher/TA perspective when the assignment is large, complex, or uncertain.

It checks:

- missed subproblems or grading points;
- formula, numeric, or logical errors;
- methods that do not match the lecture;
- text that smells too much like AI;
- whether the answer looks like something a real student would submit.

### 5. Match Previous Homework Style

College homework often rewards answering exactly what was asked: hit the scoring points, skip unnecessary explanations.

`lecture-to-hw` can read your previous Markdown submissions and learn local style habits:

- title, name, student id, and class line;
- heading depth;
- formula/table/image style;
- answer length and level of detail.

The goal is not to produce a longer tutorial. The goal is a concise, submission-ready answer.

### 6. Confidence and Manual Review Points

After finishing the workflow, `lecture-to-hw` reports:

- generated files;
- validation commands or checks;
- review changes;
- confidence level: high / medium / low;
- places that still deserve manual review.

## Installation

Clone the repository into your Codex skills directory:

```bash
mkdir -p ~/.codex/skills
git clone https://github.com/vect-G/lecture-to-hw.git ~/.codex/skills/lecture-to-hw
```

Or copy the whole `lecture-to-hw` folder to:

```text
~/.codex/skills/lecture-to-hw
```

Then start a new Codex session. The skill should be discovered automatically.

## Quick Start

In a course directory, say:

```text
Use lecture-to-hw to finish this homework and output a Markdown solution.
```

To explicitly allow parallel subagents:

```text
Use lecture-to-hw for this homework. You may split independent problems across subagents, and run a TA-style review agent after assembling the Markdown.
```

## Recommended Setup

Author-tested setup:

```text
Codex + GPT-5.5 + reasoning high + lecture-to-hw + parallel subagents allowed
```

When the assignment can be decomposed, this setup is efficient for the full loop: reading materials, splitting tasks, drafting answers, reviewing, and delivering.

If the prompt quality is poor, DOCX formulas are complex, or many questions are image-based, first convert or inspect the prompt carefully before solving.

## Repository Layout

```text
lecture-to-hw/
├── SKILL.md
├── README.md
├── README_EN.md
└── agents/
    └── openai.yaml
```

## Responsible Use

Please do a quick manual review before submitting. The skill can shoulder most of the workflow, but the course grade still belongs to you.

Hope it helps.

## License

MIT License.
