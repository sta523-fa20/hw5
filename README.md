# STA 523 :: Homework 5

> We should forget about small efficiencies, say about 97% of the time:
premature optimization is the root of all evil. Yet we should not pass up our
opportunities in that critical 3%. A good programmer will not be lulled into
complacency by such reasoning, he will be wise to look carefully at the critical
code; but only after that code has been identified.<br/><br/>
*Donald Knuth*

## Tasks

#### Task 1 - Blackjack strategy simulation

In blackjack, suppose you are dealt 8, 8. The dealer is showing a 9. Should
you "hit", "stand", or "split"? Perform a simulation using parallel computing
(up to 4 cores) to evaluate this decision. What is the probability you "win",
"push", or "lose" the hand given the three decisions?

Simulation assumptions:

- Assume one dealer and one player.
- Assume it is always the start of a new shoe (set of 7 card decks) and you
  have 8, 8; the dealer shows a card valued at 9.
- If you choose to "hit", you will continue to "hit" until you reach at least
  17.
- The dealer will always "hit" until 17 or above is reached.
- If after a "split", you have 8, 8, then you will "split" again.
- In the event of a "split", if more hands are won than lost, this is a "win".
  If more hands are lost than won, it is a "loss". Otherwise, it is a push.
- Aces count as 1 or 11. If one of the possible values gives you at least 17,
  then you must "stand". For example, if you "split" and get 8, Ace, this
  counts as 9 or 19. In this case you must take Ace as 11 and stand per the
  assumption above. The same holds for the dealer.
  
These assumptions are not an explanation of the rules of blackjack. If you do
have questions on the game, feel free to ask the instructor. Be sure to run
enough simulation replicates so you obtain an accurate result. Communicate your
result with a table or words.

#### Task 2 - Benchmarking

Use `bench::mark()` to evaluate the performance of the following sets of
functions. Provide a written summary of your results along with a visualization
or table-like object to communicate your findings.

1. Compare `apply(X, 1, sum)` and `rowSums(X)`, where `X` is a p x p random
   normal matrix. Consider values of p = 10, 100, 1,000, and 10,000. Use 10
   iterations in your performance evaluation.

2. Compare `any(x == 55)` and `55 %in% x`, where `x` is a random integer
   vector of length n. Consider values of n = 10, 100, 1,000, and 10,000. Use
   10,000 iterations in your performance evaluation. *Hint:* `sample()`.

3. Compare `t(X) %*% X` and `crossprod(X)`, where `X` is a p x p random
   normal matrix. Consider values of p = 10, 100, 1,000. Use the
   `bench::mark()` default arguments. *Note:* use `crossprod()` from package
   `Matrix`.

4. Compare `cummean(x)` from package `cumstats` with a function you develop
   to also compute the cumulative mean for a vector `x`. Let `x` be a random
   integer vector of length n. Consider values of n = 10, 100, 1,000, 10,000,
   and 100,000. Use 10,000 iterations in your performance evaluation.
   *Hint:* `sample()`.

#### Task 3 - Improving performance

Assume you want to analyze some gene expression data based on `p` = 100,000
genes and `n` = 70 individuals. Assume the individuals belong to one of two
groups.

```r
p <- 100000
n <- 70

X <- matrix(rnorm(p * n, 12, 4), nrow = p, ncol = n) # p x n matrix
group_levels <- rep(0:1, each = n / 2)
```

For each variable (gene) you want to perform a two-sample t-test and only
retain the test statistic. A `for`-loop with `t.test()` can accomplish this.

```r
system.time({
  t_stat <- vector(mode = "double", length = nrow(X))
  for (i in seq(nrow(X))) {
    t_stat[i] <- t.test(X[i, ] ~ group_levels)$statistic
  }
})
```

```
   user  system elapsed
 64.143   1.383  65.578
```

Improve the performance of this process without parallelization.
A correct solution with a run time of less than one second is guaranteed to
receive full credit. Think about the linear model example provided in the
lecture notes. Once you have optimized this process, profile your code and
briefly discuss the results.

## Essential details

### Deadline and submission

**The deadline to submit Homework 5 is Friday, November 6 at 11:59pm EST.**
Only your final commit and code in the master branch will be graded.
To submit, push your work to your assigned team repository on GitHub before
the deadline.

### Help

- Post your questions in the #hw5 channel on Slack. Explain your error / problem
  in as much detail as possible or give a reproducible example that generates
  the same error. Make use of the code snippet option available in Slack. You
  may also send a direct message to the instructor or TAs.

- Visit the instructor or TAs in Zoom office hours.

- The instructor and TAs will not answer any questions about this assignment
 	within six hours of the deadline.

### Academic integrity

This is a team assignment. You may **not** communicate with other teams in the
course. As a reminder, any code you use directly or as inspiration must be
cited.

To uphold the Duke Community Standard:

- I will not lie, cheat, or steal in my academic endeavors;
- I will conduct myself honorably in all my endeavors; and
- I will act if the Standard is compromised.

Duke University is a community dedicated to scholarship, leadership, and
service and to the principles of honesty, fairness, respect, and accountability.
Citizens of this community commit to reflect upon and uphold these principles in
all academic and non-academic endeavors, and to protect and promote a culture of
integrity. Cheating on exams and quizzes, plagiarism on homework assignments and
projects, lying about an illness or absence and other forms of academic
dishonesty are a breach of trust with classmates and faculty, violate the Duke
Community Standard, and will not be tolerated. Such incidences will result in a
0 grade for all parties involved as well as being reported to the University
Judicial Board. Additionally, there may be penalties to your final class grade.
Please review Duke’s Standards of Conduct.

### Grading

| **Topic**                           | **Points** |
|-------------------------------------|-----------:|
| Task 1                              |         13 |
| Task 2                              |          7 |
| Task 3                              |         10 |
| **Total**                           |     **30** |

*A portion of the points for each task will be allocated to code style.*

*Documents that fail to knit after minimal intervention will receive a 0*.
