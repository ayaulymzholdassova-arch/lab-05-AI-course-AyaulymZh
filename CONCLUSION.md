# Lab 05 — Conclusion

<!--
Read this before you write.

This file is graded by your instructor, by hand, and it is worth 6 of the 15 points.

- Type it yourself. Do not use a chatbot, an AI assistant, an AI autocomplete or a "rewrite my
  text" tool for any part of it, not even for grammar. English that is plain and a little
  clumsy but yours is better than polished text that is not.
- Use your own numbers. Every number you quote must be one your own notebook printed. Copy
  the number exactly as it appeared. A number your notebook did not print is marked as wrong.
- Answer in your own words, about 80-200 words per question. A short answer that uses your
  numbers beats a long answer that could have been written by anyone.
- Delete these comment lines if you like. Keep the three headings and the last line.
-->

## 1. Which layer changed your results the most?

Give the numbers before -> after for that layer (correct / wrong / no answer, out of 20) and say
whether it changed what the agent *can do* or only what it *admits*. If no layer changed the
number of correct answers, say so and explain what the layers did change instead.

Layer 1 changed the results the most. The number of correct answers increased from 16 to 18 out of 24. Wrong answers increased from 4 to 6, while unanswered questions decreased from 4 to 0.
Layer 1 allowed the agent to recover from tool errors by sending error messages back to the model. This changed what the agent could do because it could try to correct invalid tool calls instead of stopping immediately.
Layer 2 did not change the results. Layer 3 kept the number of correct answers at 18, but reduced wrong answers from 6 to 4 and increased unanswered questions from 0 to 2. It mainly changed what the agent admitted by checking whether answers were supported by tool results.

## 2. One failure the full harness did not fix

Pick one question that the full harness (layer 3) still gets wrong or refuses. Give its id
(q01 ... q20). Open its trace in the notebook: at which step did it go wrong, and what did
the model do instead? Name one thing you would add to the harness, and what it would cost
(extra model calls, tokens, time, or a new way to fail).

Question q18 was not successfully answered even with all three layers enabled.
The model retrieved the CPI values for Jambyl from 2022 to 2025. It then calculated (16.1 - 10.6) / 100 and received 0.055 from the calculation tool. However, it gave the final answer as 0.55 percentage points.
The harness detected that 0.55 was not supported by the tool results. It asked the model to correct the answer, but the model repeated the same calculation. Finally, the harness returned UNVERIFIED.
I would add an independent calculation check to the harness. It could verify whether the arithmetic and units are correct before accepting the final answer. This would improve reliability, but it would require additional calculations, tokens, and execution time.

## 3. Workflow or agent for the monthly ranking?

Dala Insight wants to publish, every month, "which region is highest and which is lowest on one
indicator in one year". Would you ship the fixed workflow or the agent? Use the accuracy, the
number of model calls, the tokens and the time you measured in Part 1. Then say in one sentence
what kind of question would make you choose the other one.


I would choose the fixed workflow for monthly regional rankings because it is more accurate, faster, and cheaper than the bare agent on fixed-shape questions.
The workflow answered 13 out of 15 questions correctly, while the agent answered only 9 out of 15 correctly.
The workflow used 15 model calls, 3,852 tokens, and 15.9 seconds. In comparison, the agent used 26 model calls, 19,835 tokens, and 46.4 seconds.
Therefore, the workflow is a better choice for regular monthly ranking tasks because the question structure is predictable and the process can follow predefined steps.
However, I would choose an agent for open-ended questions that require multiple tools, flexible decisions, or more complex reasoning.

---

I wrote this file myself, without any AI tool.
