# Vuetify Audio Player

Vuetify Material Design HTML 5 audio player, built on Vue and Vuetify

## Features

- Wrapper on HTML 5 audio component
- Compatible with Vuetify's dark mode.
- Mobile friendly compact mode.
- Play/pause, fast forward/rewind (5s), next/previous track, volume control
- Draggable and clickable seeker
- Album art, track title, track subtitle
- Support multiple icon providers (default is Material Design Icons)

### Default mode

<img src="https://i.imgur.com/LLFtCjEg.png" alt="Default mode">

### Compact mode

<img src="https://i.imgur.com/34jOjY3.png" alt="Compact mode">

### Installation

Use npm: `npm i @woodydark/vuetify-audio-player`

### Prepare

Ensure you have Vue installed and Vuetify setup:

1. Install Vuetify:

```
npm install vuetify --save-dev
```

2. Add Vuetify to `app.js` or `main.js`:

```js
import Vuetify from "vuetify"
import "vuetify/dist/vuetify.min.css"

Vue.use(Vuetify)
```

You also can use Vue plugin to install `Vuetify` by only one line command:

```
vue add vuetify
```

Node: Make sure you are using the default Vuetify icon font (mdi).

### Usage

Add below code into your `<script>`:

```js
export default {
  components: {
    VAudioPlayer: () => import("@woodydark/vuetify-audio-player"),
  },
}
```

And below code in the `<template>`:

```html
<v-audio-player
  src="http://www.example.com/your_music.ogg"
  track-title="Your Music"
  track-subtitle="Best of Electro Swing"
  allow-previous
  allow-next
  :compact="$vuetify.breakpoint.smAndDown"
  album-art="https://www.example.com/my-beautiful-art.jpeg"
  @next-audio="nextSrc()"
  @previous-audio="prevSrc()"
  prev-track-icon="mdi-skip-previous"
  next-track-icon="mdi-skip-next"
  back-forward-icon="mdi-rewind-5"
  fast-forward-icon="fast-forward-5"
  play-icon="mdi-play"
  pause-icon="mdi-pause"
  mute-volume-icon="mdi-volume-off"
  low-volume-icon="mdi-volume-low"
  medium-volume-icon="mdi-volume-medium"
  high-volume-icon="mdi-volume-high"
></v-audio-player>
```

### Props

- **src** (String): URL to audio track
- **album-art** (String): URL to audio album art. Optional.
- **track-title** (Boolean, default: false): Track title
- **track-subtitle** (Boolean, default: false): Track subtitle. Optional.
- **allow-previous** (Boolean, default: false): Determines whether "Next song" button should be disabled
- **allow-next** (Boolean, default: false): Determines whether "Previous song" button should be disabled
- **compact** (Boolean, default: false): Display the audio player in a mobile-friendly mode
- **prev-track-icon** (String, default: "mdi-skip-previous" ): Icon to play the previous track
- **next-track-icon** (String, default: "mdi-skip-next" ): Icon to play the next track
- **back-forward-icon** (String, default: "mdi-rewind-5" ): Icon to play backforward from the current track
- **fast-forward-icon** (String, default: "mdi-fast-forward-5" ): Icon to play backforward from the current
- **play-icon** (String, default: "fa-play" ): Icon to play the track
- **pause-icon** (String, default: "fa-pause" ): Icon to pause the track
- **mute-volume-icon** (String, default: "mdi-volume-off" ): Icon for the muted volume
- **low-volume-icon** (String, default: "mdi-volume-low" ): Icon for the low volume
- **medium-volume-icon** (String, default: "mdi-volume-medium" ): Icon for the medium volume
- **high-volume-icon** (String, default: "mdi-volume-high" }: Icon for the high volume

Note: The component uses Vuetify's VCard as the root template and will accept any VCard props as well.

### Events

- **previous-audio**: Fires when "previous song" button is clicked. You need to update the "src" prop manually to change the track.
- **next-audio**: Fires when "next song" button is clicked. You need to update the "src" prop manually to change the track.
- **error**: Fires when HTML Audio elements fire onError event.

### License

MIT
