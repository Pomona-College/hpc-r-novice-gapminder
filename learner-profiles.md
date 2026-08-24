---
title: Learner Profiles
---

## Who Should Take This Workshop?

Workshop 8 — *R for Reproducible Scientific Analysis* — assumes you know basic R syntax (variables, vectors, functions, control flow) and now want to organise that knowledge into project-shaped, reproducible analyses you can hand to a journal, a co-author, or your future self. The workshop leans heavily on the *tidyverse* (`dplyr`, `tidyr`, `ggplot2`), R Markdown, and the `here`/`renv` ecosystem.

The three personas below are typical of who enrolls. They all use the Sagehen HPC cluster either as their primary R environment (via the OnDemand RStudio Server app at [https://ondemand.hpc.pomona.edu](https://ondemand.hpc.pomona.edu)) or as the place where their long-running analyses end up. Wherever the lesson refers to "your R session," you can read "the OnDemand RStudio Server session running on Sagehen's amd partition."

If you have not used R before at all, take Workshop 7 (*Programming with R*) first.

---

## Profile 1: Dr. Lin — Postdoc Preparing a Reproducible Analysis for Journal Submission

### Background
Dr. Lin (she/her) is a second-year postdoctoral fellow in the Pomona College psychology department, working on a longitudinal study of executive function in undergraduate athletes. She is preparing to submit her first solo-authored manuscript to *Psychological Science*, which now requires a deposit of fully reproducible analysis code. Her PI's previous lab worked in SPSS; she taught herself R during her PhD and has been writing increasingly long single-file scripts since then. She knows the analyses are correct because she has triple-checked them, but she also knows her code is "spaghetti": one 800-line script, manual file paths, no project structure, and figures pasted into Word.

### What She Knows
- Tidyverse basics (`dplyr::filter`, `dplyr::mutate`, `ggplot2`)
- Linear mixed-effects models with `lme4`
- `read_csv`, `write_csv` and basic plotting
- How to write a function (with one or two arguments)
- The general shape of an R Markdown document (she has knit one once)

### What She Doesn't Know Yet
- How `here::here()` makes a script portable across machines (and across her laptop and Sagehen)
- How to split a long script into logical R Markdown sections that knit reproducibly end to end
- How `renv` records package versions so a reviewer running her code two years from now gets the same numbers
- How to write a `Makefile` (or `targets` pipeline) so the figures regenerate automatically when the data changes
- How to publish her work as a GitHub repo + Zenodo DOI

### Why She Needs This
- **Her journal requires it.** *Psychological Science*'s reproducibility checklist is real and is checked.
- **Her PI is watching.** If she nails this submission, she goes on the next R01 as a co-investigator.
- **Her data is on Sagehen.** The longitudinal data lives at `/bigdata/lab/psychlab/longitudinal2024/` and a full reanalysis takes ~6 hours — too long for a laptop, but a sensible SLURM batch job on the **amd** partition.

### How She Will Use Sagehen
Dr. Lin runs interactive analyses on the Sagehen OnDemand RStudio Server (8 cores, 64 GB RAM), but submits the final knit as a SLURM batch job (`sbatch knit_paper.sh`, which calls `Rscript -e "rmarkdown::render('paper.Rmd')"`). All data lives under `/bigdata/lab/psychlab/`; her R code lives in a Git repo synced to Pomona's institutional GitHub.

### Success Indicator
By the end of the workshop, Dr. Lin can:

- Restructure her 800-line script into a project with `data/`, `R/`, `reports/`, and `figures/` subdirectories
- Use `here::here()` so her code runs unchanged on her MacBook *and* on Sagehen
- Knit a single R Markdown file end to end that produces every figure and number in the manuscript
- Capture her exact package versions in a `renv.lock` file
- Push the repository to GitHub and tag a `v1.0-submission` release

### How to Pace for Dr. Lin
She is sophisticated but anxious; she wants to know that the things you are teaching are the *minimum* she needs to satisfy *Psychological Science*, and she will push back on anything that feels like polish-for-its-own-sake. Tie every concept to her submission deadline.

---

## Profile 2: Professor Alvarez — Faculty Member Teaching Reproducible Methods

### Background
Professor Alvarez (he/him) is a tenured associate professor in Pomona's economics department. He has used R for fifteen years but mostly as a "fancy calculator": one script per chapter, `setwd()` calls scattered everywhere, copies of the data in three folders. Pomona has just adopted a new methods requirement for the economics major: every senior thesis must include a fully reproducible analysis. Professor Alvarez has been volunteered (kindly but firmly) to teach the new junior-year methods course in the spring. He wants to learn the modern reproducible-analysis stack himself before he asks 22 anxious juniors to learn it from him.

### What He Knows
- A great deal of base R and statistical methods
- Stata, SAS, and Eviews from earlier in his career
- LaTeX and Beamer
- The basic shape of GitHub (he has a personal account; he has never opened a pull request)
- Excellent statistical and pedagogical instincts

### What He Doesn't Know Yet
- The tidyverse — he has read about `dplyr` but always reaches for `apply` and `[`
- R Markdown — he knit one document in 2017 and gave up
- How to teach students to use `here::here()` instead of `setwd("C:/Users/Carlos/Desktop/")`
- How to use Sagehen for class assignments (each student needs a working environment)
- `renv` — he is willing to learn it but skeptical of "yet another package manager"

### Why He Needs This
- **He is teaching this in the spring.** The course is on the books; the syllabus is due in eight weeks.
- **His students will use Sagehen.** With 22 students all needing R 4.5.3, RStudio, and identical package versions, the OnDemand RStudio Server on Sagehen is the only sane delivery model.
- **He cares about his discipline.** Several recent retraction scandals in economics have involved unreproducible Stata code; he genuinely wants his students to do better.

### How He Will Use Sagehen
Professor Alvarez will set up a course shared directory at `/bigdata/lab/economics/methods201/` with the IPUMS-CPS extracts and the assignment templates. Each student will have their own `~/methods201-USERNAME/` Git-backed project directory. He'll show them how to launch RStudio Server through OnDemand on the amd partition with modest resources (`--cpus-per-task=2 --mem=8G --time=4:00:00`).

### Success Indicator
By the end of the workshop, Professor Alvarez can:

- Convert one of his recent papers' analyses into a tidyverse pipeline + R Markdown report
- Use `here::here()` and explain *why* it matters (in language a junior with a stats minor will understand)
- Capture a course-wide `renv.lock` so all 22 students start the semester on identical package versions
- Walk a student through cloning the assignment repo on Sagehen, running the report, and pushing back changes
- Articulate the "minimum reproducible unit" he will require for each assignment

### How to Pace for Professor Alvarez
He has a pedagogical lens on everything — every concept needs to be teachable, not just learnable. Pause regularly to ask, "How would you explain this to a junior?" He will appreciate the question even if he doesn't always have an answer.

---

## Profile 3: Sam — Staff Statistician Supporting Multiple Research Groups

### Background
Sam (they/them) is a research statistician on Pomona's Research Computing team, embedded part-time across the chemistry, biology, and neuroscience departments. They have a master's in biostatistics and have been programming professionally for ten years — initially in Stata and SAS, then increasingly in R. Their job is to drop into a lab with a panicked PI three days before a grant deadline and *make the analysis work*. The variety is the hard part: in any given month, Sam might write a meta-analysis pipeline for chemistry, a survival-analysis Shiny app for biology, and a Bayesian power calculation for neuroscience. Each group has its own data conventions and its own definition of "the analysis is done."

### What They Know
- Excellent base R, tidyverse, `data.table`, and `Stan`
- Project management and time estimation under pressure
- Git, GitHub, GitLab, and pull requests
- Linux, SLURM, and Sagehen's storage layout (they completed Workshops 0–2 in 2023)
- Several reproducibility frameworks — they know `renv`, `groundhog`, and `targets` exist but don't yet have strong opinions

### What They Don't Know Yet
- Which of those reproducibility tools they should *standardise* across the labs they support
- How to set up a turnkey "new project" template that any of their PIs can spin up on Sagehen in five minutes
- How to teach R Markdown to a wet-lab PI who has never opened a code editor
- The right way to package up a finished analysis for archival on Pomona's institutional repository or Zenodo

### Why They Need This
- **They want a standard.** Right now every project they hand off has slightly different conventions. They want a single "this is how we do reproducible analysis at Pomona" pattern they can offer to every lab.
- **They want a teaching artifact.** The same template, polished, becomes the foundation for the workshops and office hours they run for graduate students and postdocs.
- **They want to retire technical debt.** Their three-year archive of analyses-for-other-people is full of `setwd` calls and unversioned packages. Modernising the older ones in the next six months is a stated goal in their performance review.

### How They Will Use Sagehen
Sam already lives on Sagehen. Their typical day is an interactive session on the amd partition (16 cores, 64 GB), with longer-running Stan models or simulations submitted as SLURM batch jobs. They use `/bigdata/lab/sam/` as their staging area; finished analyses migrate to the relevant lab's `/bigdata/lab/<labname>/` directory.

### Success Indicator
By the end of the workshop, Sam can:

- Articulate a single, defensible Pomona-standard project structure (folders, `here::here()`, `renv`, R Markdown)
- Create a `usethis::create_package()` or `cookiecutter`-style template repository other labs can fork
- Hand a wet-lab PI a one-page "How to run my analysis on Sagehen" cheat sheet that actually works
- Submit a knit-the-paper SLURM job for a former client and verify it produces identical figures
- Write a short memo to the Pomona ITS director recommending which tools to standardise on

### How to Pace for Sam
They will be the most advanced learner in the room. Engage them as a co-teacher: ask them how *they* explain `here::here()` to a chemistry PI, and have them critique the workshop's recommendations. They will bring the others up faster than you can.

---

## Common Threads

Despite very different career stages, all three personas share four things:

1. **They already know R.** This is not a "first programming language" workshop. If you are not comfortable with vectors, functions, and control flow, take Workshop 7 first.
2. **Their stakes are real.** A journal submission, a syllabus deadline, a portfolio of half-finished analyses. Pacing matters; pure-theory tangents do not.
3. **They will use Sagehen.** Long-running analyses, shared course directories, and large data on `/bigdata` push every one of them onto the cluster eventually. Treat OnDemand RStudio Server as the default environment, not an afterthought.
4. **They want a defensible standard.** Each of them is, in their own way, trying to retire technical debt. The workshop's success is measured in how confident they leave about *what* a reproducible analysis looks like, not just *how* to write one.

## What Comes Next

After Workshop 8, all three personas are well prepared for:

- **Workshop 9 — SLURM Job Scheduling on Sagehen**, particularly relevant for Dr. Lin and Sam, whose pipelines belong in batch jobs
- **Workshop 11 — OnDemand Portal Orientation**, which Professor Alvarez should take and then assign to his methods students
- **The Pomona Reproducibility Office Hours** run by Sam, which Dr. Lin will attend during the manuscript-revision phase

Cecil Sagehen approves of all of the above.

<!-- highlight <labname>/<myusername> placeholders in code blocks; remove if the varnish theme handles this natively -->
<script>(function(){var CSS='.sh-placeholder{color:#c2410c;font-weight:700}[data-bs-theme="dark"] .sh-placeholder,html.dark .sh-placeholder{color:#fdba74}@media (prefers-color-scheme: dark){[data-bs-theme="auto"] .sh-placeholder{color:#fdba74}}';var RX=/<labname>|<myusername>/g;function firstMatch(el){var w=document.createTreeWalker(el,NodeFilter.SHOW_TEXT,null),nodes=[],full='';while(w.nextNode()){nodes.push({n:w.currentNode,s:full.length});full+=w.currentNode.nodeValue;}RX.lastIndex=0;var m;while((m=RX.exec(full))){var s=m.index,e=s+m[0].length,inSpan=false,parts=[];for(var j=0;j<nodes.length;j++){var ns=nodes[j].s,ne=ns+nodes[j].n.nodeValue.length;if(ne<=s||ns>=e)continue;parts.push({node:nodes[j].n,a:Math.max(s-ns,0),b:Math.min(e-ns,nodes[j].n.nodeValue.length)});var p=nodes[j].n.parentNode;while(p&&p!==el){if(p.classList&&p.classList.contains('sh-placeholder')){inSpan=true;break;}p=p.parentNode;}}if(!inSpan&&parts.length)return parts;}return null;}function wrapParts(parts){for(var i=parts.length-1;i>=0;i--){var t=parts[i].node,txt=t.nodeValue,a=parts[i].a,b=parts[i].b;var span=document.createElement('span');span.className='sh-placeholder';span.textContent=txt.slice(a,b);var f=document.createDocumentFragment();if(a>0)f.appendChild(document.createTextNode(txt.slice(0,a)));f.appendChild(span);if(b<txt.length)f.appendChild(document.createTextNode(txt.slice(b)));t.parentNode.replaceChild(f,t);}}function run(){var st=document.createElement('style');st.textContent=CSS;document.head.appendChild(st);document.querySelectorAll('pre,code').forEach(function(el){var guard=0,parts;while((parts=firstMatch(el))&&guard++<500){wrapParts(parts);}});}if(document.readyState==='loading'){document.addEventListener('DOMContentLoaded',run);}else{run();}})();</script>
