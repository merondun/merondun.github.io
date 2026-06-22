---
layout: default
title: Week 2: Markdown & Github project organization
permalink: /tutorials/markdown_github_week2
---



# Week 2: Markdown & Github project organization

Bioinformatic projects can become an absolutely nightmare when documentation is scattered across old folders, forgotten individual scripts, and files named things like:

```text
final_analysis_v3_REAL_final_revised2.R
```

This tutorial introduces a simple documentation workflow using:

- **Typora** or another Markdown editor
- **Markdown notebooks** for long-term project organization
- **GitHub repositories** as clean project archives
- a completely **web-based GitHub workflow**

The goal is to create a project repository that looks broadly like this example:

```text
https://github.com/merondun/artocarpus_pangenome
```

By the end of this tutorial, you should be able to create a clean GitHub repository with:

- a clear `README.md`
- numbered project folders
- simple Markdown documentation
- reusable code blocks
- a structure that other people can understand

No prior coding experience is required.

___



# Recommended Setup

Before starting, please ensure you have: 

## GitHub Account

Create a free GitHub account:

```text
https://github.com/
```

## Markdown Editor

Recommended:

```text
https://typora.io/
```

Typora is useful because it provides a clean live-preview Markdown interface. It is especially convenient for writing notes and copying code blocks.

Other good options include:

- StackEdit: https://stackedit.io/
- MarkText
- Obsidian
- VSCode with Markdown preview

------



# Part 1: The Documentation Problem

Most research projects contain several types of information:

- input data
- metadata
- commands
- scripts
- intermediate results
- final figures
- failed attempts

If these are not organized early, you might be re-running things you already ran, or re-writing scripts that you can't find from last year! 

A good doc system should answer:

1. What was the project trying to do?
2. What data were used?
3. What analyses were run?
4. Where are the outputs?

The goal is not perfection, just that another person - future you next year or even month can understand the project without forensic excavation.

------



# Part 2: Typora as a Living Project Notebook

A useful habit is to keep one main Markdown notebook for each project.

For example:

```text
my_scripts/
├── 2026_06_artocarpus_pangenome.md
├── 2026_05_artocarpus_comparative.md
├── 2026_05_guava.md
├── 2025_11_hymenoptera.md
```

This works well if each project is relatively small and you only need one main notebook per project.

For larger or more exploratory projects, I prefer to give each project its own folder:

```my_scripts/
my_scripts/
├── artocarpus_pangenome/
│   ├── 2026_06_artocarpus_pangenome.md
│   ├── 2026_05_artocarpus_pangenome.md
│   └── 2026_03_artocarpus_pangenome.md
├── artocarpus_comparative/
│   ├── 2026_05_artocarpus_comparative.md
│   ├── 2026_02_artocarpus_comparative.md
│   └── 2026_01_artocarpus_comparative.md
├── guava/
│   └── 2026_06_guava.md
└── hymenoptera/
    ├── 2026_03_hymenoptera.md
    └── 2025_11_hymenoptera.md
```

Each month or so, copy the previous Markdown file and continue editing the new version.

For example:

```text
2026_05_artocarpus_pangenome.md
2026_06_artocarpus_pangenome.md
2026_07_artocarpus_pangenome.md
```

This creates a simple project history. Even if you later abandon an analysis, delete a section, or change your workflow, the old version is still preserved.

I cannot count how many times I have dropped an analysis completely, then months or even years later wanted to rerun part of the code for a different project. Keeping monthly Markdown notebooks makes that much easier.

If all your project notebooks are stored in one place, you can search them from the command line. For example, from Terminal on Mac, MobaXterm on Windows, or a Linux shell:

```bash
cd ~/my_scripts
grep -Rn --include="*.md" "hifiasm" .
```

This searches all Markdown files inside `my_scripts/` for the word `hifiasm`.

The output will show the file name and line number where the match occurs, making it easy to find old commands or notes.

This turns your old project notebooks into a personal command archive, which is often much faster than trying to reconstruct an analysis from memory, can allows you to find old scripts from years before very quickly! 

Monthly Markdown archiving is not a replacement for formal Git version control. Instead, it is a simple note-taking system that works well for long-term research projects, especially when combined with a clean GitHub repository at the end.



------



# Part 3: Living Notebook vs. GitHub Repository

It is useful to separate two ideas:

| Tool                       | Purpose                       |
| -------------------------- | ----------------------------- |
| Typora / Markdown notebook | messy living project notebook |
| GitHub repository          | clean project archive         |

The Typora notebook is where you keep evolving notes.

The GitHub repository is where you create a relatively clean and shareable version of the project.

In practice:

![markdown_vs_git](/images/Markdown_vs_Git.png)

------



# Part 4: Recommended Notebook Structure

A good project notebook should be organized around the **actual analysis workflow**, not around vague folders like `data/`, `scripts/`, and `results/`.

For research projects, I usually structure the Markdown notebook so that each major heading corresponds directly to a major analysis directory in the final GitHub repository. **This makes it incredibly easy to copy your `.md` notebook sections directly onto your Git repo pages!** 

For example, a project notebook for a pangenome project might begin like this:

```markdown
# Artocarpus altilis evolutionary genomics

End-to-end genome assembly and intraspecific pangenomics spanning genome QC, assembly, repeat annotation, pangenome graph inference, and domestication-history analyses.

Metadata live in `samples.info`. 

## Directory map

- [`01_qa_qc/`](01_qa_qc/) — read QC and genome size estimates.
- [`02_genome_assembly/`](02_genome_assembly/) — assembly generation, post-processing notes.
- [`03_whole_genome_alignments/`](03_whole_genome_alignments/) — initial whole-genome alignments.

## Questions

Questions or comments reach out to YOU email [@] gmail.com or make an issue here. 
```

This structure has one major advantage: the notebook and the GitHub repository use the same logic.

| Markdown notebook section                     | GitHub repository location             |
| --------------------------------------------- | -------------------------------------- |
| `#  Artocarpus altilis evolutionary genomics` | `README.md`                            |
| `##  01_qa_qc_genomescope`                    | `01_qa_qc_genomescope/README.md`       |
| `##  02_genome_assembly`                      | `02_genome_assembly/README.md`         |
| `##  03_whole_genome_alignments`              | `03_whole_genome_alignments/README.md` |

The top-level heading becomes the main GitHub `README.md`.

Each second-level heading becomes a numbered analysis directory.

This makes the notebook easy to convert into a clean repository later.

A useful pattern is:

```text
Markdown notebook section = analysis stage
GitHub folder = cleaned archive of that analysis stage
Folder README.md = polished version of the notebook notes
```

------



# Part 5: Markdown Basics

Markdown is a lightweight formatting language.

It is plain text with simple formatting rules.

## Headings

```markdown
# Main heading

## Section heading

### Subsection heading
```

Rendered:

# Main heading

## Section heading

### Subsection heading

------

## Bullet Lists

```markdown
- first item
- second item
- third item
```

Rendered:

- first item
- second item
- third item

------

## Numbered Lists

```markdown
1. first step
2. second step
3. third step
```

Rendered:

1. first step
2. second step
3. third step

------

## Bold and Italics

```markdown
**bold text**

*italic text*
```

Rendered:

**bold text**

*italic text*

------

## Links and Figures

Markdown can link to websites, sections of your document or Git repo, or image files!

### Website link

```markdown
[GitHub](https://github.com/)
```

Rendered:

[GitHub](https://github.com/)

### Section link

```markdown
[Part 5](#part-5-markdown-basics)
```

This links to:

[Part 5](#part-5-markdown-basics)

### Display a figure

```markdown
![Project overview](imgs/project_overview.png)
```

If the figure is in the same directory as the current `README.md` file, then you can also just have `![Project overview](project_overview.png)`! 

------

## Tables

If you're using  typora, inserting markdown tables couldn't be easier. If you have an excel  table, simply copy the cells and paste them! 

| sample | name     | coverage |
| ------ | -------- | -------- |
| s1     | maafala  | 34.5     |
| s2     | ulu_fiti | 23.4     |

If you're using a different editor, you can always use a web tool like [tableconvert]([Convert Markdown Table to Markdown Table Online - Table Convert](https://tableconvert.com/markdown-to-markdown)) 



------



# Part 6: Code Blocks

Code blocks are one of the most useful features of Markdown.

They allow you to store commands cleanly inside your notes.

Example:

~~~markdown
```bash
mkdir results
ls -lh
echo "hello documentation"
```
~~~

Rendered:

```bash
mkdir results
ls -lh
echo "hello documentation"
```

The word after the first three backticks tells Markdown what language the code uses, although Typora also provides a fill-box at the bottom of each codeblock where you can indicate the language. This isn't necessary, but it helps with syntax highlighting. 

------

# Why Code Blocks Are Useful

Code blocks are not just for display.

They become reusable command chunks.

In Typora, you can click inside a code block and copy the entire block easily.

Typical workflow:

```text
1. Write commands inside a fenced code block
2. Click inside the code block
3. Select and copy the full block with ctrl+a + ctrl+c (windows)
4. Paste into Terminal or VS code. 
```

------



# Part 7: Creating a GitHub Repository

Open GitHub:

```text
https://github.com/
```

Then:

1. click **New repository**
2. choose a repository name and short description
3. choose public or private and initialize with `README.md` 

Preferably for names use lowercase names and underscores or hyphens and as always...avoid spaces. 

------



# Part 8: Creating File & Folders on GitHub

To create a folder through the github web, create a file and include the folder name with `/`  before the file name.

For example:

```text
01_qa_qc/README.md
```

If you already have a Markdown notebook, copy the matching `01_qa_qc/` section and paste it into this new `README.md`, then click **Commit changes**.

GitHub will create both the directory `01_qa_qc/` and the readme: 

```text
01_qa_qc/
README.md
```

Repeat for:

```
02_genome_assembly/README.md
03_whole_genome_alignments/README.md
```

___



# Part 9: What Not to Put on GitHub

Do not upload:

- passwords, confidential data, huge files (max ~50 Mb)
- API keys, private tokens

Large raw data files are usually better on Zenodo. 

------



# Part 10: Summary

A practical long-term workflow is:

```text
1. Keep a local project folder
2. Maintain one main Markdown notebook in Typora
3. Store commands in code blocks
4. Keep notes under headings that match the GitHub repo
5. Promote cleaned sections into GitHub README files
6. Store final outputs and figures in clearly named folders
```

This creates continuity between daily work and final archiving.


