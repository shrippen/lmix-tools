# Projekt: lmix tools — Werkzeuge für NDI und OMT unter Linux

## Kontext
Unter Linux gibt es für NDI nur SDK, Discovery Server und
Kommandozeilenwerkzeuge, für OMT nur die Bibliotheken; Oberflächen fehlen
ganz. lmix tools schließt die Lücke nach dem Vorbild der NDI Tools, jedes
Werkzeug für NDI und OMT (`docs/LASTENHEFT.md`). Schwesterprojekt von lmix
(`../lmix`, Mischer und Show); beide teilen GStreamer-Elemente und Teile
der Oberfläche (Lastenheft §6.3).

Kommuniziere auf Deutsch, Code, Commits und PR-Texte auf Englisch.

## Feststehende Entscheidungen (User 2026-10-28, Lastenheft §6.1)
1. Eine Codebasis, ein Programm `lmix-tools` mit Ansichten; jede Ansicht
   auch einzeln, mehrfach und ohne Oberfläche als Dienst (T-E.1). Was ohne
   Oberfläche läuft, steckt in `lmix-toolsd` ohne Qt (T-E.8).
2. Leichtgewichtig: kein lmix-core, kein strom; GStreamer direkt (T-E.2/3).
3. NDI nur über das SDK, zur Laufzeit geladen, nie mitgeliefert. OMT über ein
   eigenes GStreamer-Element `gst-omt`, das auch lmix lädt (T-E.3, T-E.5).
4. Oberfläche: Rust + Qt Quick/Kirigami über cxx-qt wie lmix console-qt,
   alles aus Kante (`agent.md`).
5. Plattform Linux x86-64, dazu ARM64 (Raspberry Pi 5) für die leichten
   Werkzeuge (TNF.1). Kein Windows/macOS.
6. Clean Room nach TCR.1–4 (Lastenheft §0): Vorbild nur aus der öffentlichen
   NDI-Doku und den OMT-Repos; neue Anforderung → Quelle in
   `docs/LASTENHEFT-QUELLEN.md`.

## Arbeitsregeln
- Gemeinsamer Code mit lmix entsteht einmal (Lastenheft §6.3). Was beide
  brauchen und heute in lmix liegt, zieht beim ersten Bedarf hierher um; lmix
  stellt im selben Zug um (PR in beiden Repos).
- Jede Funktion für NDI und OMT; wo ein Protokoll etwas nicht kann, sagt die
  Oberfläche warum (Lastenheft §2.2).
- Tests: Kreistest Sender → Empfänger je Protokoll ohne Hardware (TNF.7);
  was ein NDI-Gerät, vMix oder eine Karte braucht, steht als Hardware-Abnahme
  im Lastenheft.
- Entscheidungen mit Datum in `docs/DECISIONS.md`.
- Demo-Modus mit den gemeinsamen Demodaten nur intern für Screenshots
  (Regel in `../CLAUDE.md`).
- Nach jedem Meilenstein (T1–T3): STOPP, Abnahme durch den User.

## Repo-Struktur (geplant, Lastenheft §6.3)
crates/
  gst-omt/          ← GStreamer-Plugin omtsrc/omtsink/Geräteanbieter
  lmix-netsource/   ← Quellen im Netz finden (NDI + OMT)
  lmix-media-qt/    ← Videofläche, Pegel, Quellenauswahl (cxx-qt + QML)
  lmix-tools-core/  ← Werkzeuge ohne Oberfläche (Pipelines, Dienst-API)
  lmix-toolsd/      ← Programm ohne Qt: Dienst, Kommandozeile, Kiosk
  lmix-tools/       ← Programm mit Oberfläche (Ansichten)
docs/               ← Lastenheft, Quellen, Entscheidungen
