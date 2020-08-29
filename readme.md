# Vuetify Audio Player
Material Design HTML 5 audio player, built on Vue and Vuetify

## Features
- Wrapper on HTML 5 audio component
- Compatible with Vuetify's dark mode.
- Mobile friendly compact mode.
- Play/pause, fast forward/rewind (5s), next/previous track, volume control
- Draggable and clickable seeker
- Album art, track title, track subtitle

### Default mode
<img src="https://i.imgur.com/LLFtCjEg.png" alt="Default mode">

### Compact mode
<img src="https://i.imgur.com/34jOjY3.png" alt="Compact mode">


### Installation

Use npm: ```npm install vuetify-audio-player --save```

### Prepare
Ensure you have Vue installed and Vuetify setup:
1. Install Vuetify:
```
npm install vuetify --save-dev
```
2. Add Vuetify to ```app.js``` or ```main.js```:
```js
import Vuetify from 'vuetify';
import 'vuetify/dist/vuetify.min.css';

Vue.use(Vuetify);
```

You also can use Vue plugin to install ```Vuetify``` by only one line command:
```
vue add vuetify
```

Node: Make sure you are using the default Vuetify icon font (mdi).

### Usage
Add below code into your ```<script>```:
```js
export default {
    components: {
        VuetifyAudioPlayer: () => import('vuetify-audio-player'),
    }
}

```

And below code in the ```<template>```:
```html
<vuetify-audio-player
    src="http://www.example.com/your_music.ogg"
    track-title="Your Music"
    track-subtitle="Best of Electro Swing"
    allow-previous
    allow-next
    :compact="$vuetify.breakpoint.smAndDown"
    album-art="https://www.example.com/my-beautiful-art.jpeg"
    @next-audio="nextSrc()"
    @previous-audio="prevSrc()"
></vuetify-audio-player>
```

### Props
 - **src** (String): URL to audio track
 - **album-art** (String): URL to audio album art. Optional.
 - **track-title** (Boolean, default: false): Track title
 - **track-subtitle** (Boolean, default: false): Track subtitle
 - **allow-previous** (Boolean, default: false): Determines whether "Next song" button should be disabled
 - **allow-next** (Boolean, default: false): Determines whether "Previous song" button should be disabled
 - **compact** (Boolean, default: false): Display the audio player in a mobile-friendly mode

 Note: The component uses Vuetify's VCard as the root template and will accept any VCard props as well.


### Events
 - **previous-audio**: Fires when "previous song" button is clicked. You need to update the "src" prop manually to change the track.
 - **next-audio**: Fires when "next song" button is clicked. You need to update the "src" prop manually to change the track.
 - **error**: Fires when HTML Audio elements fire onError event.
 
### License

MIT