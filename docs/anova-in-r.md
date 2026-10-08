# ANOVA in R: a short reference

This lab is about reproducing someone else's numbers, not about the theory of
ANOVA. That comes in PSYC 201B. This page covers just enough to read the R
output and match it to what the paper reports.

---

## Fitting the model

```r
mod <- aov(DISTANCE ~ DIRECTION * STN_NAME, data = d)
summary(mod)
```

- `DISTANCE ~ ...` reads "distance, predicted by ...".
- `A * B` expands to `A + B + A:B`: both main effects plus the interaction.
  `A:B` is the interaction alone.
- Predictors must be **factors**. If a grouping variable is stored as a number
  (like `STN_NUMBER`), `aov()` treats it as one continuous slope and you get
  1 degree of freedom where you expected 3. The `Df` column is the quickest
  place to spot this.

## Reading the output

```
                    Df Sum Sq Mean Sq F value   Pr(>F)
DIRECTION            1   0.71    0.71   0.664    0.416
...
Residuals          194 208.15    1.07
```

| Column | What it is | Where it appears in the paper |
|---|---|---|
| `Df` | degrees of freedom for this effect | first number in F(**3**, 194) |
| `Df` on the `Residuals` row | residual degrees of freedom | second number in F(3, **194**) |
| `Sum Sq` | variance explained by this effect | used to compute ηp² |
| `F value` | Mean Sq of the effect ÷ Mean Sq of the residuals | F |
| `Pr(>F)` | the *p*-value | *p* |

`tidy()` from the broom package returns the same table as a tibble, with
columns `term`, `df`, `sumsq`, `meansq`, `statistic` (F) and `p.value`. From
there you can `filter()`, `mutate()` and `select()` as with any other data.

## Partial eta squared (ηp²)

$$
\eta_p^2 = \frac{SS_\text{effect}}{SS_\text{effect} + SS_\text{residual}}
$$

It is "partial" because the other effects in the model are left out of the
denominator. In a one-way ANOVA (one predictor), there is nothing else to
leave out, so ηp² is the same as plain η² = SS_effect / SS_total.

You will see packages that compute this for you (`effectsize`, `lsr`). Doing it
by hand with `tidy()` and `mutate()` takes about two lines and shows you
exactly what the number is.

## One-way ANOVA with two groups is a *t*-test

With a single two-level predictor (east vs. west at one station), the ANOVA F
equals the squared *t* from a pooled-variance *t*-test, and the *p*-values are
identical. Try `t.test(DISTANCE ~ DIRECTION, data = st_george, var.equal = TRUE)`
and compare.

---

## Spoiler: why the station F is 23.35 in R and 24.10 in the paper

*Try bonus question 1 in the report before reading this.*

Study 1 is **unbalanced**: the cells are not all the same size (Bloor-Yonge has
23 eastbound and 26 westbound participants). When cell sizes differ, the
predictors are slightly correlated, and some variance could be credited to
either one. Statistical software has to decide who gets it, and there is more
than one convention:

- **Type I (sequential)** sums of squares, which R's `aov()` uses, credit each
  term with whatever variance is left after the terms listed *before* it. So
  the **order of the predictors in the formula changes the answer**:
  `DIRECTION * STN_NAME` and `STN_NAME * DIRECTION` give slightly different F
  values for the main effects.
- **Type III** sums of squares, the default in SPSS, test each term as if it
  had been entered last, after every other term, so the order does not matter.

The interaction is entered last either way, so it comes out identical
(16.28) in both. The main effects are what differ. The authors used SPSS, so
their F(3, 194) = 24.10 is a Type III result.

You can get Type III tests in base R. You need sum-to-zero contrasts, then
drop each term from the full model one at a time:

```r
mod_lm <- lm(DISTANCE ~ DIRECTION * STN_NAME, data = d,
             contrasts = list(DIRECTION = contr.sum, STN_NAME = contr.sum))
drop1(mod_lm, . ~ ., test = "F")
```

That gives station F(3, 194) = 24.10, matching the paper. (The `car` package's
`Anova(mod, type = 3)` does the same thing, if you meet it later.)

**The point for reproducibility:** the paper said "ANOVA" and nothing else. A
reader needs to know *which* sums of squares, and *which software*, to get the
same number, and the paper does not say. This is very common, and it is the
kind of thing your reflection should mention.
