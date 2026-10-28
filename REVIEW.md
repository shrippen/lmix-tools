# Review guide

Every PR is reviewed before it is merged (rule in `agent.md`): by PR-Agent;
a new Claude agent checks the **Wichtig** of earlier rounds, and reviews
when PR-Agent fails. The reviewer knows nothing of the session that wrote
the change: it gets the PR's intent, its diff and the repository, and this
file. The owner does not read code; this review is the second reader.

## What failure looks like here

lmix tools run next to live productions: a monitor at the director's
desk, a test pattern on a line, an audio bridge into the PA, a recorder
of every camera. A bug that matters is one that someone sees or hears,
or one that takes a signal off the network:

- **On the wire:** a sender that stops, freezes or goes black, sound
  with gaps, clicks or drift, a source under the wrong name, tally that
  lies.
- **Stops the tool:** a panic, a deadlock, a blocked streaming thread, a
  tool that does not come back after its source drops or the network
  changes.
- **Leaks over a long day:** pipelines, pads, threads, sockets or memory
  not freed when a source goes away (tools run for days).
- **Hurts the network:** a receiver that pulls full bandwidth where the
  user chose the proxy, a discovery storm, a port opened beyond what the
  tool needs.
- **Breaks lmix:** a shared crate or the GStreamer plugin changes so that
  lmix no longer builds or loads it.
- **Lies:** docs, DECISIONS, LASTENHEFT status or comments that say
  something the code does not do.

## What to check in particular

- GStreamer: closures hold `WeakRef`s, not strong refs; probes that run
  per buffer are cheap; an event is taken with `Handled` plus
  `info.data = None`, not `Drop`; no seek from a streaming thread.
- NDI: the SDK is loaded at runtime only, never linked or shipped; the
  tool says so clearly when it is missing.
- OMT: input from the network (frames, metadata XML) is untrusted.
- Root cause on bug fixes: does the fix remove the cause, or hide one case
  of it? Is there a test that failed before the fix?
- Tests: is the new behaviour tested where it can be (unit, a loop test
  sender → receiver)? Say what is only verified on hardware later.

## Skip

- Generated files, `Cargo.lock`.
- `demo/` screenshots and images.
- Formatting and style that `cargo fmt` and `clippy` already enforce.

## How to report

- Every finding names file and line and the concrete input or state that
  goes wrong. No guess without evidence; if unsure, say so.
- Each finding gets one label: **Wichtig** (a bug from the list above,
  must be fixed or answered before the merge), **Kleinigkeit** (optional,
  at most five) or **Bestand** (the bug was there before this PR; not
  blocking).
- The report starts with the count, e.g. "2 Wichtig, 1 Kleinigkeit" (the
  session puts the heading above it). The count of **Wichtig** holds every
  one still open, also from an earlier round, e.g. "1 Wichtig (offen aus
  Runde 1)". Only when none is open it says plainly: "Nichts zu beheben."
- In a later round: say for each open **Wichtig** of the last round whether
  it is fixed, or whether its answer is accepted, with the reason. PR-Agent
  sees no earlier round: it reports only its own findings with their labels,
  and a new Claude agent closes the open ones (`agent.md`).
- Findings in German, code names as they are.
