<template>
  <div id="map" :class="{ active: isLoaded }" v-if="haveCoordinates"></div>
</template>

<script>
  export default {
    name: 'background',
    computed: {
      haveCoordinates () {
        return (this.store.geocode.latitude && this.store.geocode.longitude)
      },
      isLoaded () {
        if (this.store.appStatus.state === 'loaded') {
          return true
        }
      },
      store () {
        return this.$store.state
      }
    },
    methods: {
      background () {
          /* eslint-disable no-new, no-undef */
        new google.maps.Map(document.getElementById('map'), {
          center: {lat: this.store.geocode.latitude - 1.5, lng: this.store.geocode.longitude},
          disableDefaultUI: true,
          draggable: false,
          scrollwheel: false,
          mapTypeControl: false,
          mapTypeId: 'satellite',
          navigationControl: false,
          scaleControl: false,
          zoom: 8
        })
      }
    },
    watch: {
      isLoaded () {
        (this.store.googleMapsLoaded && window.innerWidth > 550) ? this.background() : null
      }
    }
  }
</script>

<style lang="scss">
#map {
  filter: grayscale(100);
  height: 100%;
  left: 0;
  opacity: 0;
  pointer-events: none;
  position: fixed !important;
  top: 0;
  width: 100%;

  &.active {
    opacity: 0.5;
    transition: 0.3s;
  }

  > div {
    background-color: transparent !important;
  }

  @media(max-width: 550px) {
    display: none;
  }

  /* Hide Google Map extra UI elements */
  .gmnoprint a, .gmnoprint span, .gm-style-cc {
    display: none;
  }

  .gmnoprint div {
    background: none !important;
  }
}
</style>
