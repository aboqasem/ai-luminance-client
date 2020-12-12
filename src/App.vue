<template>
  <div class='start' v-on:click='getLuminance' v-bind:style='{ background: `rgb(${r},${g},${b})`, }'>
    <p v-bind:style='{color: `${isLight ? "#121212" : "#FFFFFF"}`}'>{{title}}</p>
  </div>
</template>

<script>
const serverUrl = 'https://ai-luminance.herokuapp.com/';
export default {
  name: 'App',
  created() {
    // call the server once created so it trains the neural network.
    fetch(serverUrl);
  },
  data: function() {
    return {
      title: 'Start',
      r: undefined,
      g: undefined,
      b: undefined,
      isLight: true,
    };
  },
  methods: {
    getLuminance: async function() {
      await fetch(serverUrl)
      .then(res => res.json())
      .then(json => {
        const { r, g, b, likely } = json;
        this.title = likely;
        this.r = r;
        this.g = g;
        this.b = b;
        this.isLight = likely === 'Light';
      })
      .catch(() => {
        this.title = 'Server Down 🛠';
        this.r = 176;
        this.g = 0;
        this.b = 32;
        this.isLight = false;
        console.log('Failed to fetch colors.');
      });
    },
  },
}
</script>
