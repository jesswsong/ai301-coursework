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

I am a student contributor in this repo, not a maintainer or an expert outsider. I am trying to verify a reported behavior with evidence from my own environment, and I will keep my comments short, factual, and easy for maintainers to evaluate.

## Rules I write by

### Rule: Name the environment

I only describe a bug as reproduced when I say what environment I tested it in. If the issue depends on a specific package version or platform, I call that out instead of speaking in generalities.

- Wrong: "I reproduced this on my machine."
- Right: "I reproduced this on macOS 14.5 with Python 3.11 and package version 2.4.0."

### Rule: State the observed behavior, not the diagnosis

I describe what happened in the app or tool and let the evidence carry the point. I do not turn a symptom into a confident explanation before I have proof.

- Wrong: "This is definitely caused by a broken config system."
- Right: "When I run the command above, the app throws the ValueError shown below."

### Rule: Separate cannot-reproduce from a claim

If I cannot reproduce the issue, I say that plainly and explain what I checked. I do not hide uncertainty behind a vague statement or a wrong-target conclusion.

- Wrong: "Can't reproduce, so likely not a real bug."
- Right: "I could not reproduce this in the same environment, and the issue may depend on a different setup or config."

### Rule: Keep the claim specific and scoped

I describe only what my evidence supports and avoid broad claims or summaries that overstate the problem.

- Wrong: "This feature is broken across the whole app."
- Right: "This behavior fails in the create flow when the field is empty."

### Rule: Make the next step easy for a stranger

I write comments that a teammate or maintainer could follow without my context. I state the starting point, the trigger, and the output.

- Wrong: "Same as before, just run it and you'll see it."
- Right: "Start from a clean install, run the command below, and the error appears immediately after the upload step."

## Things I never post

- "Same as above" or "can confirm" without my own reproduction.
- "This is definitely broken everywhere" without a concrete failing case.
- "Works for me" when I have not checked the issue's environment or setup.
- A major claim before I have shown the output, log, or screenshot.
- A promise like "fixed" or "resolved" before I have verified the actual behavior.
