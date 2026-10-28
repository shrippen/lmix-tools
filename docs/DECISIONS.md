# Entscheidungen

## 2026-10-28: Umfang, ein Programm, eigenes Repo

Kontext: Unter Linux fehlen die NDI Tools, und für OMT gibt es keinen
Betrachter. Der User wünscht ein Lastenheft nach dem Vorbild der NDI Tools
für NDI und OMT.

Optionen: getrennte Programme je Werkzeug, ein Programm mit Ansichten, eine
Seite in lmix.

Entscheidung (User): eine Codebasis, ein Programm mit Ansichten, jede auch
einzeln und als Dienst; leichtgewichtig ohne lmix-core; so viel Code wie
sinnvoll gemeinsam mit lmix; eigenes Repo. NDI-SDK-Nutzung ist in Ordnung.
Englische Namen der Werkzeuge. Router, Access Manager, Bridge und Remote
später. Einzelheiten: `LASTENHEFT.md` §6.
