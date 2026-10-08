# Reproducibility lab: Maglio & Polman (2014)

Lab for **PSYC 201A** (UC San Diego). You take the data the authors posted for
a published study and try to get the same numbers they reported. When you are
done you will have:

- Loaded a real, published dataset with `here()` and `read_excel()`
- Re-run the paper's main analysis (a 2 × 4 ANOVA plus four follow-up tests)
- Computed effect sizes yourself with tidyverse verbs
- A rendered report that says which results reproduced, which did not, and why
- Your work committed and pushed to GitHub

The study: Maglio, S. J., & Polman, E. (2014). Spatial orientation shrinks and
expands psychological distance. *Psychological Science, 25*(7), 1345–1352.
<https://doi.org/10.1177/0956797614530571>

People waiting for a subway train rated how far away other stations *felt*.
Did stations they were heading toward feel closer than stations they were
leaving behind?

> **Before you start:** finish
> [getting-started-with-r](https://github.com/psyc-201/getting-started-with-r).
> This lab assumes R, RStudio, GitHub Desktop, and the `tidyverse` and `here`
> packages are already installed, and that you have done one
> edit → commit → push.

---

## Part 1. Make your own copy

Same steps as in getting-started-with-r:

1. At the top of [this repository's GitHub page](https://github.com/psyc-201/reproducibility-lab),
   click the green **Use this template** button, then **Create a new repository**.
2. Owner: **your own account**. Name it `reproducibility-lab`, leave it
   **Public**, and click **Create repository**.
3. On *your* copy (the header reads `yourname/reproducibility-lab`), click
   **Code** → **Open with GitHub Desktop** → **Clone**.

If any of this is unfamiliar, the step-by-step guide is
[github-desktop.md](https://github.com/psyc-201/getting-started-with-r/blob/main/docs/github-desktop.md)
in the getting-started repository.

## Part 2. Open the project and pick a version

**Double-click `reproducibility-lab.Rproj`** to open RStudio inside the project.
That is what makes `here("data", "S1_Subway.xlsx")` find the data on your
computer.

There are two versions of the report. They have the same steps and reproduce
the same results. Pick **one**:

| File | Pick it if… |
|---|---|
| `reproducibility-report-bumper-rails.qmd` | You are newer to R. Most code is written; you replace each `"FIXME"`. |
| `reproducibility-report-minimal.qmd` | You have used the tidyverse before. You get the structure and the questions, and you write the code. |

Switching partway is fine. If you get stuck in the minimal version, look up the
same step in the bumper-rails file.

Then open `original_paper/maglio-polman-2014.pdf` and read **Study 1** (about
two pages). The report quotes the exact sentences you are trying to reproduce.

## Part 3. Work through the report

Put your name in the `author:` line, then go step by step. Run one chunk at a
time with the green arrow at its top right, and click **Render** whenever you
want to see the whole report.

Both files set `error: true`, so **Render works even while chunks are
unfinished**: the chunks that fail show their error in red in the output. When
nothing is red, you are done with the code.

The steps:

1. **Load packages:** `tidyverse`, plus `readxl` and `broom`. Both come
   with the tidyverse, so you do not need to install anything new.
2. **Load the data** with `read_excel()` and `here()`.
3. **Tidy:** make direction and station into factors.
4. **Reproduce the analysis:** cell counts, the 2 × 4 ANOVA, partial eta
   squared, and a follow-up test at each of the four stations.
5. **Reflect:** what reproduced, what did not, and what made it hard.

Then three bonus questions, one of which explains a number that does not quite
match the paper.

## Part 4. Commit, push, and submit

Commit as you go, not just at the end. A commit after each step is a good
rhythm. When you are finished:

1. Render one last time and check the HTML looks right.
2. In GitHub Desktop, commit with a message like `Finish reproducibility report`,
   then **Push origin**.
3. Submit the link to your repository however your instructor asks.

The rendered `.html` is ignored by git (see `.gitignore`), so only your `.qmd`
gets pushed. Anyone who clones your repository can render the report
themselves, and that is the point: if it renders on someone else's computer
from your repository alone, your analysis is reproducible. If your instructor wants the HTML committed too, delete the
matching lines from `.gitignore`.

## What is in this repository

```
reproducibility-lab/
├── reproducibility-lab.Rproj                 open this to start work
├── reproducibility-report-bumper-rails.qmd   scaffolded version: fill in the FIXMEs
├── reproducibility-report-minimal.qmd        write-it-yourself version
├── data/
│   ├── S1_Subway.xlsx                        Study 1, used in this lab
│   └── S2_…, S3a_…, S3b_…, S4_…, S5_…       Studies 2–5, for extra practice
├── original_paper/
│   └── maglio-polman-2014.pdf                the article
└── docs/
    ├── codebook.md                           what every column means
    └── anova-in-r.md                         reading aov() output; partial eta squared
```

The data are the authors' own files from the Open Science Framework
(<https://osf.io/7rajd/>), not simulated. The paper earned Open Data and Open
Materials badges, which is why this exercise is possible at all.

## Want more?

Studies 2–5 are in `data/` too. Pick one, find its results paragraph in the
paper, and try to reproduce it with the same workflow. The column names are
listed in [docs/codebook.md](docs/codebook.md).

## Stuck?

1. **Setup errors** (package not found, file does not exist, `here()` in the
   wrong place): see
   [troubleshooting.md](https://github.com/psyc-201/getting-started-with-r/blob/main/docs/troubleshooting.md)
   in the getting-started repository. The fixes are the same. Swap in
   `reproducibility-lab.Rproj` wherever it says `getting-started-with-r.Rproj`.
2. **`could not find function "read_excel"` or `"tidy"`:** you skipped
   `library(readxl)` or `library(broom)`. Run the packages chunk again.
3. **Your ANOVA has 1 degree of freedom where the paper has 3:** station is
   being treated as a number. Check Step 3 and
   [docs/codebook.md](docs/codebook.md).
4. **Questions about the output:** [docs/anova-in-r.md](docs/anova-in-r.md).
5. Still stuck? Post the **exact** error message in the course forum.

## Credits

Adapted from the reproducibility exercise in Stanford's Psych 251 and the
earlier UCSD Psych 201a
[problem sets](https://github.com/ucsd-psych201a/problem_sets) (Janna Wennberg,
Khuyen Le, and Mihir Gujarathi). This version uses Quarto, the tidyverse, and
`here()` throughout.
