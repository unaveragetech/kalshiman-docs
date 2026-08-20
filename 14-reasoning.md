# Why three layers — and why the middle one is an LLM

## The original bot had one brain, and the brain was a formula

Here's what the old system did, honestly: it looked at a snapshot of numbers,
ran them through a curve, and output "probably up." Every round, the same
way. It didn't know how the market had gotten here, didn't know gold at 8pm
behaves differently than gold at noon, and didn't know that a dip on a
high-confidence pick is usually just noise. It was a thermostat with a
thermometer — it reacted to the number in front of it and had no idea what the
number meant.

The losses it produced weren't random. They were structural. You can't fix a
formula's blindness by tuning the formula; you need something that can *see*.

## Watch a good trader for ten minutes

They don't compute. They observe. They glance at the book, watch how the
candles are stacking, notice the volume picking up, remember that this series
has been reversing late all evening — and then they say "yeah, I'll take that"
or "no, that's a trap." That's reasoning over context. It's the thing no
feature vector captures, because it lives in the *shape* of the situation, not
the numbers.

So we added a reasoning model — a small language model that runs locally — to
play that role. Every round, it gets the same snapshot a trader would look at:
the implied price, the book, the trend path, the series history. And it
answers, in a sentence, "yes" / "no" / "pass — this one's a trap." Its answers
are written down and scored against what actually happens, exactly like a
trader's journal.

That's the piece that makes the system feel like it has a person watching it.
Because it does — a cheap, tireless one.

## But the observer can't be trusted alone

The language model's instincts are real and also noisy. In testing it said
"YES, 85%" about a market that was priced at 3% — confident, articulate, and
completely wrong. A bot that trades on hunches is a bot that overtrades. The
observer is the *eye*, never the *hand*.

## The hand is a discipline problem, not a math problem

The last layer is the one that wins or loses the game: knowing when to act,
and what to do when a trade goes sideways. The rules sound simple, and they
are:

- Only act when the statistician is confident **and** the observer agrees.
- Once in, let a dip breathe for three minutes before you even consider
  leaving.
- If it's still down after three minutes, leave — at the stop level, not at
  whatever price the market panicked to.

Every one of those rules exists because a specific losing pattern taught it
to us. The three-minute grace exists because nine of fifteen stops in the old
system recovered and won. The stop-level fill exists because a 15-cent stop
once booked a 50-cent loss. This is a system that keeps its receipts.

## The balance, in one sentence

**A calibrated probability, an observer's judgment, and a disciplined hand —
held together by a journal that never lies.** Too hot and the noise eats you.
Too cold and you never win. Just right is a boring hour: eight trades, eight
wins, eleven cents each, and a good night's sleep.
