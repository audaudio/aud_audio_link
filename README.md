# aud_audio_link

English | [Deutsch](README.de.md)

[![CI](https://github.com/audaudio/aud_audio_link/actions/workflows/quick_check.yaml/badge.svg)](https://github.com/audaudio/aud_audio_link/actions/workflows/quick_check.yaml)

Ableton Link transport provider of the Audanika Audio Engine; fetches Link and LinkKit at build time, app publishers license Link themselves.

Part of the Audanika Audio Engine; planned in
[aud_audio_pm](https://github.com/audaudio/aud_audio_pm). The package is a
scaffold so far: it builds its native code on iOS, Android, macOS, Windows
and Linux, but none of the features of its description is implemented yet.

## Motivation

The transport of the Audanika Audio Engine takes its tempo and beat from
a transport provider of `aud_audio_graph`. Ableton Link lets apps on one
network share tempo, beat and phase; this package is the place for a
provider that follows Link. The Link sources are not part of the repo:
they are fetched at build time, and whoever publishes an app with it
licenses Link with Ableton themselves. The repo was created with the other
repos of the engine and holds the boilerplate of a native Dart package;
the provider follows in later tickets.

## Installation

The package is not published to pub.dev. Add it as a git dependency,
resolved by its version tag:

```yaml
dependencies:
  aud_audio_link:
    git:
      url: git@github.com:audaudio/aud_audio_link.git
      tag_pattern: "{{version}}"
    version: ^0.0.3
```

Then run `dart pub get`. The native library is built by the build hook of
the package when the app is built.

## Usage

The library exports the two functions of the native package template, a
short-lived one and a long-running one that runs on a helper isolate. The
example in `example/` shows them in a Flutter app:

```dart
import 'package:aud_audio_link/aud_audio_link.dart';

Future<void> main() async {
  // A short-lived native function, called on the main isolate
  print('sum(24, 18) = ${sum(24, 18)}');

  // A long-running native function, called on a helper isolate
  print('await sumAsync(24, 18) = ${await sumAsync(24, 18)}');
}
```

It prints:

```text
sum(24, 18) = 42
await sumAsync(24, 18) = 42
```

## Limitations

- The Link transport provider is not implemented yet, and the build hook
  does not fetch Link or LinkKit yet; the library holds the template
  functions `sum` and `sumAsync` only
- Ableton Link is licensed by Ableton; the package does not grant that
  license, the publisher of an app obtains it
- The native code needs a native platform; the package does not compile
  for the web

## Documentation

- [Blog](doc/blog) — one post per change
- [aud_audio_pm](https://github.com/audaudio/aud_audio_pm) — the plans and
  decisions of the Audanika Audio Engine

## Status

Version 0.0.3, a scaffold. Before 1.0 — the API may change between minor
versions.

## Contributing

- Follow the [develop guide](doc/guides/develop-guide.md), the
  [code guide](doc/guides/code-guide.md) and the
  [test guide](doc/guides/test-guide.md)
- Regenerate the bindings with `dart run ffigen --config ffigen.yaml`
- The example resolves together with the package in a pub workspace, so
  that upgrading the package upgrades the example too

## License

MIT — see [LICENSE](LICENSE).
