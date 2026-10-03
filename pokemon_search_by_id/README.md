# Pokemon Search by ID (Android)

Look up a Pokemon by its numeric ID against the live PokeAPI and display its
sprite, name, and cry sound.

## What it does

- Enter a Pokemon ID and tap search; the app fetches that Pokemon from
  `https://pokeapi.co/api/v2/pokemon/{id}`.
- On the result screen it shows the Pokemon's sprite image and name, and
  plays its cry sound automatically.
- A return button sends you back to search again.

## Tech stack

Java, Android SDK, `HttpURLConnection` for the network call, `org.json` for
parsing, `SharedPreferences` to hand the parsed result off between screens,
`MediaPlayer` to stream and play the cry audio.

## How it works

`ApiCall` (a `Thread` subclass) opens an `HttpURLConnection` to PokeAPI, reads
the JSON response, and pulls out `sprites.front_default`, `name`, and
`cries.latest` (falling back to `cries.legacy` if empty), writing them to
`SharedPreferences`. `MainActivity`'s search button starts that thread and
joins it before launching the `Pokemon` results activity, which reads the
stored values back, downloads the sprite image directly into a `Drawable`,
and streams the cry URL into a `MediaPlayer`.

**Known limitation:** the network call currently runs synchronously on the
UI thread (`apiCall.start(); apiCall.join();`), which can briefly freeze the
UI on a slow connection — a background-thread/callback refactor would be the
next improvement.

## Build & run

Open the project root in Android Studio and run the `app` configuration on
an emulator or device with internet access (Gradle-managed, no extra setup
required).
