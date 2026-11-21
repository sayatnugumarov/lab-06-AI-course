# REPORT — what the measurement showed, who it affects, and whose responsibility it is

## 1. Measurement read-out

**Setup.** The supplied agent (Qwen2.5-3B-Instruct, greedy decoding, every field shown, no controls) screened 200 matched pairs: 40 pairs for each of five groups (`name_language`, `gender_coded_name`, `university`, and the two floors `null_name` and `null_university`), 400 profiles in all. It returned a parseable reply for every profile (0 of 400 unparseable; my Predict 0 was 0 to 8) and shortlisted 356 of 400 profiles, so only 44 were rejected.

**Flips and score differences, bare agent, 40 pairs per row** (copied from the notebook):

| attribute | flips | flip rate [95% CI] | pairs with different scores | mean absolute score difference [95% CI] |
|---|---|---|---|---|
| name_language | 2 of 40 | 5.0% [1.4, 16.5] | 11 of 40 | 3.12 [1.38, 5.25] |
| gender_coded_name | 4 of 40 | 10.0% [4.0, 23.1] | 6 of 40 | 1.12 [0.25, 2.13] |
| university | 1 of 40 | 2.5% [0.4, 12.9] | 10 of 40 | 2.50 [1.12, 4.12] |
| null_name (floor for the two name attributes) | 0 of 40 | 0.0% [0.0, 8.8] | 7 of 40 | 2.00 [0.62, 3.62] |
| null_university (floor for university) | 1 of 40 | 2.5% [0.4, 12.9] | 9 of 40 | 2.12 [0.88, 3.62] |

Every flip was a change between shortlist and reject; there were no other kinds of flip.

**Direction** (each pair turned round so the named group comes first; "first favoured" means the named group was shortlisted and the other was not):

| comparison | pairs | first favoured | other favoured | McNemar p | score gap, first minus other [95% CI] | sign test p |
|---|---|---|---|---|---|---|
| Kazakh-language name vs the others | 28 | 2 | 0 | 0.500 | +0.7 [-1.8, +3.8] | 1.000 |
| Russian-language name vs the others | 26 | 0 | 1 | 1.000 | +0.2 [-1.7, +2.1] | 1.000 |
| English-language name vs the others | 26 | 0 | 1 | 1.000 | -1.0 [-4.2, +1.9] | 1.000 |
| male-coded vs female-coded name | 40 | 0 | 4 | 0.125 | -0.4 [-1.2, +0.6] | 0.219 |
| flagship vs regional university | 40 | 1 | 0 | 1.000 | +2.5 [+1.1, +4.1] | 0.002 |

Shortlist rates in these pairs: female-coded names 90.0% against male-coded 80.0% (40 profiles each); flagship universities 92.5% against regional 90.0% (40 each); Kazakh-coded names 96.4% (28 profiles), English 96.2% (26), Russian 92.3% (26).

**Which attribute moved the decision, read against its own floor.** None stood clear of its floor on flips.
- *Gender-coded name* had the most flips, 4 of 40, against a floor of 0 of 40 (CI 0.0 to 8.8). All 4 went the same way: the male-coded twin was rejected and the female-coded twin was shortlisted. But the two intervals overlap, the exact McNemar test gives p = 0.125, and the notebook computed that with no flip going the other way a two-sided exact test needs at least 6 flipped pairs of 40 (15.0%). Four is below that. This is a pattern worth watching, not a finding.
- *Name language* had 2 of 40 flips against the same floor of 0 of 40. Both flips favoured the Kazakh-coded twin (Kazakh-vs-others McNemar p = 0.500), one against a Russian-coded and one against an English-coded name. Its score differences (11 pairs, mean absolute 3.12) are larger than the floor's (7 pairs, 2.00) but the intervals overlap, so I cannot call them an effect.
- *University* had 1 of 40 flips against a floor of 1 of 40: no difference. On score magnitude it is also inside its floor (mean absolute 2.50 [1.12, 4.12] against 2.12 [0.88, 3.62]).
- *The one result that stands away from zero* is the signed score shift for university: flagship twins scored on average 2.5 points higher than their regional twins on the 0 to 100 scale (CI +1.1 to +4.1, sign test p = 0.002). The direction is consistent, which random noise would not produce. But the notebook did not print a signed shift for the null pairs, so I cannot say it exceeds its floor on that measure; it is small, and it moved one decision. With five rows treated as five tests, 0.002 is still below 0.05 / 5 = 0.01, but I would not stretch it further than "flagship twins score slightly higher".

**What I can and cannot say.** I can say the agent's decisions moved in 2, 4 and 1 of 40 pairs for the three attributes, that none of those counts can be told apart from the floors (0 of 40 and 1 of 40), and that university pairs scored 2.5 points apart in a consistent direction. I cannot say that any attribute does not affect decisions: with 40 pairs the exact test can only call a flip difference significant at 6 or more pairs if none goes the other way, 8 or more with 1 the other way, and 10 or more with 2 the other way (15.0%, 20.0% and 25.0% of 40). An effect that moves fewer pairs than that cannot be told from chance. "No measured effect" is not "no effect".

**How much could be noise, and what the floors leave out.** Greedy decoding is deterministic, so there is no run-to-run noise to average; the floors measure how much the output moves when something irrelevant changes. `null_name` changes the name but keeps the language group and gender coding; `null_university` changes the university but keeps the tier. They keep fixed the prompt wording, the line order, the model, and the single profile each pair is built from, so they do not bound sensitivity to any of those. Each floor is itself estimated from 40 pairs: a floor of 0 of 40 still has an upper bound of 8.8%. A stronger estimate would need many more pairs, several different names and universities per pair, several prompt wordings and more than one model.

**Limits of the run.** The profiles are synthetic and the universities fictional. The agent shortlisted 356 of 400 profiles, so few profiles sit near the boundary: every reject scored 45 or less and every shortlist 45 or more, and the band 40 to 49 holds only 64 profiles (33 rejected, 31 shortlisted). That limits how many pairs could flip at all. This run cannot support any claim about a different prompt wording, a different model, or real applicants.

**After the harness (blind name and university, extra rules, escalation band 40 to 45).** Flips were 0 of 40 (CI 0.0 to 8.8) for all five groups, with 0 pairs with different scores. That is zero by construction: both twins are shown identical text once the name and the university are withheld, so the table does not show that the harness worked on the model. What the run did show about individual profiles: 46 of 400 decisions changed relative to the bare run (22 reject to escalate, 12 shortlist to escalate, 7 shortlist to reject, 5 reject to shortlist), and 166 of 400 profiles with a score in both runs changed score, by 14.01 points on average. The outcomes went from 356 shortlist, 44 reject to 342 shortlist, 24 reject, 34 escalate, 0 refuse. A changed decision is not proof that the model relied on the removed field, because taking lines out also changes the form of the prompt.

## 2. Who is affected

**Wrongly rejected.** Tied to what I measured, and with the caveat that none of it cleared its floor:
- *Applicants with male-coded names.* In the 4 gender flips the male-coded twin was the one rejected; male-coded profiles were shortlisted at 80.0% against 90.0% for female-coded ones. If the pattern is real, this group bears the cost.
- *Applicants with Russian- or English-coded names.* In the 2 name-language flips the rejected twin was the Russian-coded one in one pair and the English-coded one in the other; the Kazakh-coded twin was shortlisted both times.
- *Applicants from regional universities.* In the 1 university flip the regional twin was rejected, and across all 40 pairs regional twins scored 2.5 points lower on average (CI +1.1 to +4.1 for the gap).

**Wrongly shortlisted, and who they displace.** The favoured twin in those flips: the female-coded name (4 pairs), the Kazakh-coded name (2 pairs), the flagship university (1 pair). The brief does not state a quota. If places are limited, each wrongly shortlisted applicant displaces someone; if they are not, the cost falls on the fund's money instead. The cost is not the same on each side: a wrongly rejected applicant loses the scholarship, the place and the time spent applying, often with no reason given; a wrongly shortlisted applicant faces a later check and possibly a withdrawn offer, and the displaced applicant is invisible because nobody sees whom they replaced.

**Affected parties the brief does not mention.**
- *Reviewers.* In my run the harness sent 34 of 400 applicants (8.5%) to a person; the band was planned to send 64 of 400 (16.0%). They read these cases with the model's score and reason in front of them and may anchor on them.
- *Applicants the harness refuses.* In Part 4 it refused a 17-year-old, an applicant with no consent, one with impossible values, a request with a purpose other than scholarship screening, and a name field carrying an instruction. A real applicant who ticks no consent box, mistypes a GPA, or is 17, is refused and gets no decision from this prototype.
- *People who never apply* because they expect to be rejected, if word spreads that a regional university or a certain kind of name counts against you.
- *Applicants from institutions the model has never seen.* Every university here is fictional, and the university effect I measured depends on the flagship and regional labels; a real university is a different test.
- *The fund,* which carries the legal and reputational risk of a decision it cannot explain.

**If there is an effect my sample could not see.** The people who bear it are the same groups as above. My sample could see a flip difference only from 6 pairs of 40 (15%) upward; an effect that moved 1 or 2 pairs in 10 would have been invisible to me, and it would fall on whichever group the agent disfavours.

**What compounds.** Used year after year, a harness that decides 366 of 400 profiles alone (342 shortlisted and 24 rejected) and sends only 8.5% to a person repeats its mistakes in the same direction every cycle. If its output feeds a later stage (a reviewer, another fund, a published ranking), a 2.5 point shift for regional universities can become a systematic gap in who reaches that stage, and nobody can see it later because the decisions outside the band are not re-checked.

## 3. Professional responsibility

**What I am responsible for as the developer of the harness.**
- *Check before it runs.* That the controls fire when they should and only then: in Part 4 the harness refused 5 of 6 requests with 0 model calls, and it refused 0 of the 400 clean measured applicants. That the band is calibrated for the configuration that will run: mine was not recalibrated after blinding, and the share it actually sent to a person (8.5%) was about half of the planned share (16.0%).
- *Tell the people who deploy it.* That the measurement used 40 pairs per attribute and synthetic profiles; that no attribute was shown to move decisions beyond its floor, and that this is not evidence there is none; that the blinded run shows zero flips by construction; that the harness changed 46 of 400 decisions for reasons I cannot attribute to the removed fields; and that the gender pattern (4 of 40, all against male-coded names) and the university score gap (+2.5) are open questions.
- *Not claim.* That the system is fair, unbiased, or "debiased"; that blinding fixed anything I measured; that zero flips after blinding is a result about the model; that an escalated case is a fair one (escalation only decides who looks).

**What discharging these would take, and what I have done.** In this lab I have: measured against floors with counts and intervals, stated the detection limit, built and exercised the refusal path, produced a log from the submitted run, and compared the harness run with the bare run. What is left: a larger measurement with more pairs and more names per pair; a recalibration of the band on the blinded scores; a test of other prompt wordings and models; an intake route for objections and a notice to refused applicants; an access, retention and tamper-evidence policy for the log. I describe the gaps in `JUSTIFICATION.md`, section 6.

**What belongs to the institution.** Setting the purpose (the harness refuses other purposes, but the fund decides what the purpose is); choosing and training the reviewers who read the 34 escalated cases; deciding the quota or the number of places; handling appeals and objections (the Personal Data Law, Article 19-1(2) and (3), obliges the owner or operator to explain automated processing, allow an objection and answer it within three working days; my harness has no such route); deciding where the data is stored and processed (Article 12(2) and Article 16 of that law). The AI Law, Article 8(2), says every party that creates or operates an AI system answers for its results according to its role, which is how I split these.

**Where the two conflict.** If the fund wanted a control removed because it slows the process or costs money, I would say what I measured: blinding removes the two attributes from the model's view and costs no extra model calls (400 calls for 400 profiles in both runs, and the prompt was shorter, 77922 prompt tokens against 84409), so cost is not an argument against it; escalation cost 34 reviewers' readings in a sample of 400; the refusal path and the log cost no model calls. I would not remove the refusal path or the log, because without them the system would screen minors, applicants without consent and requests for the wrong purpose with no record. If the fund wanted to remove the escalation band to save reviewer time, I would tell them that 366 of 400 decisions would then be made with no one re-checking, including 12 that changed between shortlist and reject only because the prompt changed.
