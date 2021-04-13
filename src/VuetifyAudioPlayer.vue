<template>
  <v-card
    class="mt-5"
    ref="playerContainer"
    :loading="!audioDownloaded"
    v-bind="$attrs"
  >
    <v-img
      v-if="albumArt && compact"
      :src="albumArt"
      aspect-ratio="1"
      contain
      class="grey darken-3"
    ></v-img>

    <audio
      ref="audio"
      @pause="playing = false"
      @play="playing = true"
      @timeupdate="handleTimeUpdate"
      @durationchange="setDuration"
      @canplaythrough="audioDownloaded = true"
      @ended="handleAudioEnd"
      @error="$emit('error', $event)"
      :src="src"
    ></audio>

    <v-slider
      class="audio-seeker"
      v-if="src"
      min="0"
      max="1000000"
      :value="parseInt((currentTime / duration) * 1000000) || 0"
      @input="seek($event)"
      @focus="seekerFocused = true"
      @blur="seekerFocused = false"
    ></v-slider>

    <v-card-text>
      <v-row
        :class="compact ? 'text-center' : 'text-left'"
        align="center"
        justify="center"
      >
        <v-col :cols="compact ? 12 : 6" class="d-flex align-center">
          <v-avatar tile class="d-inline-block" v-if="albumArt && !compact">
            <v-img :src="albumArt" aspect-ratio="1"></v-img>
          </v-avatar>

          <div
            class="mx-auto"
            :class="albumArt && !compact && 'ml-3 d-inline-block'"
          >
            <span v-if="trackTitle" class="d-block" v-text="trackTitle"></span>
            <span
              v-text="trackSubtitle"
              class="d-block text-uppercase font-weight-bold"
              style="letter-spacing: 0.05em"
            ></span>
          </div>
        </v-col>

        <v-spacer></v-spacer>

        <v-col :cols="compact ? 12 : 2">
          <div
            class="d-flex align-center mx-auto"
            :class="compact ? 'justify-center' : 'justify-end'"
            style="max-width: 12rem"
          >
            <v-btn icon @click="muted = !muted">
              <v-icon v-text="volumeIcon"></v-icon>
            </v-btn>

            <v-slider
              class="mt-2 volume-slider"
              :value="muted ? 0 : volume"
              @input="setVolume"
              thumb-label
              max="100"
              min="0"
            ></v-slider>
          </div>
        </v-col>

        <v-col
          v-if="src"
          :cols="compact ? 12 : 4"
          class="d-flex align-center"
          :class="compact ? 'justify-center' : 'justify-end'"
        >
          <div :class="compact ? 'mx-1' : 'mx-2'">
            <v-btn
              icon
              :disabled="!audioDownloaded || !allowPrevious"
              @click="$emit('previous-audio')"
            >
              <v-icon size="20">{{ prevTrackIcon }}</v-icon>
            </v-btn>
          </div>

          <div :class="compact ? 'mx-1' : 'mx-2'">
            <v-btn
              icon
              :disabled="!audioDownloaded"
              @click="forwardSeconds(-5)"
            >
              <v-icon size="20">{{ backForwardIcon }}</v-icon>
            </v-btn>
          </div>

          <div :class="compact ? 'mx-2' : 'mx-3'">
            <v-btn
              icon
              :disabled="!audioDownloaded"
              @click="playing = !playing"
            >
              <v-icon
                size="30"
                v-text="playing ? pauseIcon : playIcon"
              ></v-icon>
            </v-btn>
          </div>

          <div :class="compact ? 'mx-1' : 'mx-2'">
            <v-btn icon :disabled="!audioDownloaded" @click="forwardSeconds(5)">
              <v-icon size="20">{{ fastForwardIcon }}</v-icon>
            </v-btn>
          </div>

          <div :class="compact ? 'mx-1' : 'mx-2'">
            <v-btn
              icon
              :disabled="!audioDownloaded || !allowNext"
              @click="$emit('next-audio')"
            >
              <v-icon size="20">{{ nextTrackIcon }}</v-icon>
            </v-btn>
          </div>
        </v-col>
      </v-row>
    </v-card-text>
  </v-card>
</template>

<script>
export default {
  props: {
    src: { type: String },
    trackTitle: { type: String },
    trackSubtitle: { type: String, default: undefined },
    allowPrevious: { type: Boolean, default: false },
    allowNext: { type: Boolean, default: false },
    compact: { type: Boolean, default: false },
    albumArt: { type: String, default: undefined },
    autoplay: { type: Boolean, default: false },
    // icons
    prevTrackIcon: { type: String, default: "mdi-skip-previous" },
    nextTrackIcon: { type: String, default: "mdi-skip-next" },
    backForwardIcon: { type: String, default: "mdi-rewind-5" },
    fastForwardIcon: { type: String, default: "mdi-fast-forward-5" },
    playIcon: { type: String, default: "mdi-play" },
    pauseIcon: { type: String, default: "mdi-pause" },
    muteVolumeIcon: { type: String, default: "mdi-volume-off" },
    lowVolumeIcon: { type: String, default: "mdi-volume-low" },
    mediumVolumeIcon: { type: String, default: "mdi-volume-medium" },
    highVolumeIcon: { type: String, default: "mdi-volume-high" },
  },
  data() {
    return {
      audioDownloaded: false,
      currentTime: 0,
      duration: 0,
      playing: false,
      volume: 20,
      seekerFocused: false,
      keydownListener: null,
      muted: false,
    };
  },
  watch: {
    playing(value) {
      if (value) {
        return this.$refs.audio.play();
      }
      this.$refs.audio.pause();
    },
    muted(value) {
      this.$refs.audio.muted = value;
    },
    audioDownloaded(value) {
      if (this.autoplay) {
        if (value) {
          this.playing = true;
        }
      }
    },
    src(value) {
      if (value) {
        this.audioDownloaded = false;
        this.playing = false;
      }
    },
    volume() {
      this.muted = false;
    },
  },
  computed: {
    volumeIcon() {
      if (this.muted) {
        return this.muteVolumeIcon;
      } else if (this.volume === 0) {
        return this.lowVolumeIcon;
      } else if (this.volume >= 50) {
        return this.highVolumeIcon;
      } else {
        return this.mediumVolumeIcon;
      }
    },
  },
  methods: {
    setVolume(value) {
      this.volume = value;
      this.$refs.audio.volume = value / 100;
    },
    forwardSeconds(seconds) {
      let newTimestamp = this.currentTime + seconds;

      if (newTimestamp < 0) {
        newTimestamp = 0;
      } else if (newTimestamp > this.duration) {
        newTimestamp = this.duration;
      }

      this.$refs.audio.currentTime = newTimestamp;
    },
    setDuration() {
      this.duration = this.$refs.audio.duration;
    },
    handleTimeUpdate() {
      this.currentTime = this.$refs.audio.currentTime;
    },
    handleAudioEnd() {
      if (this.allowNext) {
        this.$emit("next-audio");
      }
    },
    seek(timePercent) {
      this.$refs.audio.currentTime =
        this.$refs.audio.duration * (timePercent / 1000000.0);
    },
  },
  mounted() {
    this.$refs.audio.volume = this.volume / 100;
    this.muted = this.$refs.audio.muted;

    this.keydownListener = document.addEventListener("keydown", (event) => {
      if (event.keyCode === 32 && this.seekerFocused) {
        event.preventDefault();
        this.playing = !this.playing;
      }
    });
  },
  beforeDestroy() {
    document.removeEventListener("keydown", this.keydownListener);
  },
};
</script>

<style lang="scss">
.volume-slider .v-messages {
  display: none;
}

.audio-seeker {
  .v-slider {
    min-height: 0;
  }

  .v-slider--horizontal {
    margin-left: 0;
    margin-right: 0;
  }

  .v-slider__track-background {
    width: 100% !important;
  }

  .v-messages {
    display: none;
  }

  .v-slider__thumb:before {
    opacity: 0;
  }

  .v-slider__thumb {
    height: 10px;
    width: 10px;
    cursor: pointer;
  }

  .v-slider__track-container {
    cursor: pointer;
    height: 6px !important;
  }

  .v-slider__track-fill,
  .v-slider__track-background,
  .v-slider__track-container {
    border-radius: 9999px;
  }

  * {
    transition: none !important;
  }
}
</style>
