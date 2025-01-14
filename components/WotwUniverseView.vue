<template>
  <v-card class='universe-view'>
    <v-sheet v-if='!hideColor' :color='universe.color' height='0.5em' />
    <v-card-title>
      <div>
        {{ universe.name }}
        <copyable-info v-if="devtoolsEnabled" :value="universe.id" />
      </div>
    </v-card-title>
    <v-card-text>
      <v-scroll-y-reverse-transition leave-absolute group tag='div' class='d-flex flex-wrap worlds'>
        <wotw-world-view
          v-for='world in universe.worlds'
          :key='world.id'
          class="flex-grow-1"
          :world='world'
          :disabled='disabled'
          :can-join='canJoin'
          :multiverse-id='multiverseId'
          :seed='seed'
          @join='$emit("join-world", world.id)'
        />
      </v-scroll-y-reverse-transition>
    </v-card-text>
    <v-btn v-if='canJoin && canCreateWorld' :disabled='disabled' block color='accent' tile @click='$emit("new-world")'>
      New World
    </v-btn>
  </v-card>
</template>

<script>
  import {mapState} from 'vuex'

  export default {
    name: 'WotwUniverseView',
    props: {
      universe: {
        type: Object,
        required: true,
      },
      disabled: {
        type: Boolean,
        default: false,
      },
      canJoin: {
        type: Boolean,
        default: true,
      },
      canCreateWorld: {
        type: Boolean,
        default: true,
      },
      multiverseId: {
        type: Number,
        required: false,
        default: null,
      },
      hideColor: {
        type: Boolean,
        required: false,
        default: false,
      },
      seed: {
        type: Object,
        required: false,
        default: null,
      },
    },
    computed: {
      ...mapState('user', ['user']),
      ...mapState('dev', ['devtoolsEnabled']),
    },
  }
</script>

<style lang='scss' scoped>
  .universe-view {
    min-width: 15vw;
    overflow: hidden;
  }

  .worlds {
    gap: 1em;
  }
</style>
