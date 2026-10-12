# Voice guide: how I talk upstream

<!--
THIS IS THE PART YOU WRITE (new this week). Live mode reads this file
before any comment of yours goes out the door; eval mode ignores it
entirely, because your voice is yours and carries no gold labels.

This is not etiquette. "Be polite and concise" is advice for everyone
and therefore rules for no one. Write rules YOU need, in your own
words, each one concrete enough that the skill can hold a draft
against it and say which rule it breaks.

Three sections. Fill all three.
-->

## Who I am in threads

<!-- 2-3 lines. Who is talking when you comment on an issue: your
experience level stated plainly, what you are doing in this repo, what
readers can expect from you. This is the register your rules protect. -->
I am a newcomer contributor working on targeted bug fixes and test improvements in this repository. I write directly, technically, and concisely, focusing on verifiable evidence, reproducible test steps, and clear terminal outputs rather than broad guarantees.

## Rules I write by

<!-- 3-5 rules, drafted from the lecture's slide-12 moment. Each rule
needs a wrong/right pair from your own hand: one line you might
actually have written that breaks the rule, and the line you would
post instead. The pair is what makes a rule executable; a rule without
one is a wish.

Format each rule like this:

### Rule: <short name>

<The rule, one or two sentences.>

- Wrong: "<a line that breaks it>"
- Right: "<the line to post instead>"
-->

### Rule: Promise investigation, never a fix

State the intent to investigate and reproduce the behavior locally; never guarantee that a fix will be delivered or merged before investigating.

- Wrong: "I will fix this bug by tomorrow and submit a PR."
- Right: "I'd like to investigate this issue and see if I can reproduce the behavior locally with the test suite."

### Rule: Factual reproduction, no hand-waving

Report exact environment versions, test commands, and verbatim failure logs rather than summarizing with subjective adjectives.

- Wrong: "I ran the test and it clearly threw a bunch of errors as expected."
- Right: "Running `pytest -k test_verify_password_malformed_hash` on Python 3.11 (WSL2/Ubuntu) produces `UnknownHashError` as shown in the traceback below."

### Rule: Humble collaboration, no presumptuous authority

Offer findings for review and maintainer alignment rather than prescribing sweeping architectural mandates.

- Wrong: "The implementation in this module is wrong and needs to be completely rewritten."
- Right: "Looking at the error trace, handling `UnknownHashError` to fail closed appears to match the expected authentication contract. Let me know if this direction aligns with repository preferences."

## Things I never post

<!-- A short list. Promises you cannot keep, tones you refuse,
shortcuts you know you reach for when tired. The skill quotes this
list back at you when a draft crosses it. -->

- Unprompted ETAs or artificial completion deadlines (e.g., "Will finish by Friday").
- Claims of reproduction without pasting the environment and command output.
- Piggybacking on previous comments ("Same here" or "+1 can confirm") without presenting independent evidence.
- Presumptuous solution claims before reproducing the failing behavior.
