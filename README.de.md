# aud_audio_link

[English](README.md) | Deutsch

[![CI](https://github.com/audaudio/aud_audio_link/actions/workflows/quick_check.yaml/badge.svg)](https://github.com/audaudio/aud_audio_link/actions/workflows/quick_check.yaml)

Ableton-Link-Transport-Provider der Audanika Audio Engine; holt Link und LinkKit zur Build-Zeit, App-Publisher lizenzieren Link selbst.

Teil der Audanika Audio Engine; geplant in
[aud_audio_pm](https://github.com/audaudio/aud_audio_pm). Das Paket ist
bisher ein Gerüst: Es baut seinen nativen Code auf iOS, Android, macOS,
Windows und Linux, aber keine der Funktionen seiner Beschreibung ist schon
umgesetzt.

## Motivation

Der Transport der Audanika Audio Engine bezieht Tempo und Beat von einem
Transport-Provider von `aud_audio_graph`. Ableton Link lässt Apps in
einem Netz Tempo, Beat und Phase teilen; dieses Paket ist der Ort für
einen Provider, der Link folgt. Die Link-Quellen sind nicht Teil des
Repos: Sie werden zur Build-Zeit geholt, und wer eine App damit
veröffentlicht, lizenziert Link selbst bei Ableton. Das Repo wurde mit
den anderen Repos der Engine angelegt und enthält das Gerüst eines
nativen Dart-Pakets; der Provider folgt in späteren Tickets.

## Installation

Das Paket ist nicht auf pub.dev veröffentlicht. Füge es als
Git-Abhängigkeit hinzu, aufgelöst über seinen Versions-Tag:

```yaml
dependencies:
  aud_audio_link:
    git:
      url: git@github.com:audaudio/aud_audio_link.git
      tag_pattern: "{{version}}"
    version: ^0.0.3
```

Dann `dart pub get` ausführen. Die native Bibliothek baut der Build-Hook
des Pakets, wenn die App gebaut wird.

## Verwendung

Die Bibliothek exportiert die zwei Funktionen der Vorlage für native
Pakete, eine kurzlebige und eine langlaufende, die auf einem Hilfs-Isolate
läuft. Das Beispiel in `example/` zeigt sie in einer Flutter-App:

```dart
import 'package:aud_audio_link/aud_audio_link.dart';

Future<void> main() async {
  // A short-lived native function, called on the main isolate
  print('sum(24, 18) = ${sum(24, 18)}');

  // A long-running native function, called on a helper isolate
  print('await sumAsync(24, 18) = ${await sumAsync(24, 18)}');
}
```

Es gibt aus:

```text
sum(24, 18) = 42
await sumAsync(24, 18) = 42
```

## Grenzen

- Der Link-Transport-Provider ist noch nicht umgesetzt, und der Build-Hook
  holt Link und LinkKit noch nicht; die Bibliothek enthält nur die
  Vorlagenfunktionen `sum` und `sumAsync`
- Ableton Link wird von Ableton lizenziert; das Paket gewährt diese Lizenz
  nicht, der Publisher einer App holt sie ein
- Der native Code braucht eine native Plattform; das Paket kompiliert
  nicht fürs Web

## Dokumentation

- [Blog](doc/de/blog) — ein Beitrag pro Änderung
- [aud_audio_pm](https://github.com/audaudio/aud_audio_pm) — die Pläne und
  Entscheidungen der Audanika Audio Engine

## Status

Version 0.0.3, ein Gerüst. Vor 1.0 — die API kann sich zwischen
Minor-Versionen ändern.

## Mitwirken

- Folge dem [Develop-Guide](doc/guides/develop-guide.md), dem
  [Code-Guide](doc/guides/code-guide.md) und dem
  [Test-Guide](doc/guides/test-guide.md)
- Die Bindings neu erzeugen mit `dart run ffigen --config ffigen.yaml`
- Das Beispiel löst sich zusammen mit dem Paket in einem Pub-Workspace
  auf, damit ein Upgrade des Pakets auch das Beispiel aktualisiert

## Lizenz

MIT — siehe [LICENSE](LICENSE).
