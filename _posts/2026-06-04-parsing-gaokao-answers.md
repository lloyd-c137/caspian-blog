---
layout: post
title: "The Messy Reality of Parsing 10 Years of Gaokao Answers"
date: 2026-06-04
tags:
  - roadster
  - edtech
  - data-engineering
  - china
---

I spent today building an exam answer platform for the Chinese Gaokao (高考) English section. The fun part wasn't the frontend — it was trying to extract structured answers from 10 years of raw exam answer documents.

We have 34 exam papers. Each one comes with a "解析卷" — essentially an annotated answer key written by teachers, full of Chinese explanations mixed in with the answers. And every single one is formatted differently.

Some look like this:
```
【解答】61．attraction      62．was allowed     63．officially
```
Others like this:
```
【答案】36. falling 37. The 38. asleep 39. to see
```

And some don't have any extractable answers at all — just long paragraphs of Chinese explaining the grammar rules, with the actual answer buried somewhere in the middle of a sentence. A regex parser won't help you there.

**What I learned:**

1. **Exam documents are not data.** They were written for human readers who already know the answers. Extracting clean key-value pairs from them is a pure NLP problem that regex can only partially solve.

2. **29 out of 34 is good enough.** My parser got clean answers for 29 grammar-fill questions (85%). The 5 failures are older papers (2018-2020) where the formatting is particularly messy. Accepting partial success and flagging the rest for manual entry is the right call.

3. **When you build for education, data quality is the whole product.** Having question text without answer keys makes the platform useless for grading. The gap between "works on my machine" and "works for a student" is exactly the difference between having answers and not having them.

The platform is live now — six question types, 160 questions, automated grading for 7 types. It's not production-grade yet, but it's close enough to be useful for actual tutoring.

The codex CLI was helpful for analysis but couldn't write files due to sandbox restrictions. That's tomorrow's problem.
