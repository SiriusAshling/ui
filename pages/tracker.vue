<template>
  <div class='fill-height' :class='{"electron-draggable": isElectron}'>
    <v-fade-transition mode='out-in'>
      <div v-if='connected && receivedPacket' key='tracker' class='tracker-container' :class='{done: showDone}'>
        <div class='tracker pa-2'>
          <WotwTrackerSkillView skill='spike' :active='trackedValues.skill_spike' />
          <WotwTrackerSkillView skill='sentry' :active='trackedValues.skill_sentry' />
          <WotwTrackerSkillView skill='blaze' :active='trackedValues.skill_blaze' />
          <WotwTrackerSkillView skill='flap' :active='trackedValues.skill_flap' />
          <WotwTrackerSkillView class='clean-water' skill='clean_water' :active='trackedValues.skill_clean_water' />
          <WotwTrackerResourceView
            class='resource-view'
            :finished="trackedValues.game_finished"
            :flags='seedFlags'
            :spirit-light='trackedValues.resource_spirit_light'
            :gorlek-ore='trackedValues.resource_gorlek_ore'
            :keystones='trackedValues.resource_keystones'
            :show-willow-hearts='showWillowHearts'
            :total-tree-count='14'
            :total-wisp-count='5'
            :total-quest-count='17'
            :total-relic-count='trackedValues.relic_count_total'
            :total-heart-count='8'
            :tree-count='trackedValues.tree_count'
            :wisp-count='trackedValues.wisp_count'
            :quest-count='trackedValues.quest_count'
            :relic-count='trackedValues.relic_count'
            :heart-count='heartCount'
          />
          <WotwTrackerSkillView skill='hammer' :active='trackedValues.skill_hammer' />
          <WotwTrackerSkillView skill='shuriken' :active='trackedValues.skill_shuriken' />
          <WotwTrackerSkillView skill='water_breath' :active='trackedValues.skill_water_breath' />
          <WotwTrackerSkillView skill='glide' :active='trackedValues.skill_glide' />

          <WotwTrackerSkillView skill='sword' :tree='trackedValues.tree_sword' :active='trackedValues.skill_sword' />
          <WotwTrackerSkillView skill='bow' :tree='trackedValues.tree_bow' :active='trackedValues.skill_bow' />
          <WotwTrackerSkillView skill='bash' :tree='trackedValues.tree_bash' :active='trackedValues.skill_bash' />
          <WotwTrackerSkillView skill='dash' :tree='trackedValues.tree_dash' :active='trackedValues.skill_dash' />
          <WotwTrackerSkillView skill='water_dash' :tree='trackedValues.tree_water_dash' :active='trackedValues.skill_water_dash' />
          <WotwTrackerSkillView skill='burrow' :tree='trackedValues.tree_burrow' :active='trackedValues.skill_burrow' />
          <WotwTrackerSkillView skill='ancestral_light_glades' :tree='trackedValues.tree_ancestral_light_glades' :active='trackedValues.skill_ancestral_light_glades' />
          <WotwTrackerSkillView skill='double_jump' :tree='trackedValues.tree_double_jump' :active='trackedValues.skill_double_jump' />
          <WotwTrackerSkillView skill='regenerate' :tree='trackedValues.tree_regenerate' :active='trackedValues.skill_regenerate' />
          <WotwTrackerSkillView skill='grapple' :tree='trackedValues.tree_grapple' :active='trackedValues.skill_grapple' />
          <WotwTrackerSkillView skill='launch' :tree='trackedValues.tree_launch' :active='trackedValues.skill_launch' />
          <WotwTrackerSkillView skill='flash' :tree='trackedValues.tree_flash' :active='trackedValues.skill_flash' />
          <WotwTrackerSkillView skill='light_burst' :tree='trackedValues.tree_light_burst' :active='trackedValues.skill_light_burst' />
          <WotwTrackerSkillView skill='ancestral_light_marsh' :tree='trackedValues.tree_ancestral_light_marsh' :active='trackedValues.skill_ancestral_light_marsh' />
        </div>
        <div v-if="showTeleporters" class='teleporters'>
          <WotwTrackerTeleporterView style="grid-column: 3; grid-row: 1;" name='Wellspring' :active='trackedValues.tp_wellspring' />
          <WotwTrackerTeleporterView style="grid-column: 5; grid-row: 1;" name='Reach' :active='trackedValues.tp_baurs_reach' />
          <WotwTrackerTeleporterView style="grid-column: 6; grid-row: 1;" name='Shriek' :active='trackedValues.tp_willow_outer' />
          <WotwTrackerTeleporterView style="grid-column: 1; grid-row: 2;" name='Pools West' :active='trackedValues.tp_luma_pools_west' />
          <WotwTrackerTeleporterView style="grid-column: 2; grid-row: 2;" name='Pools East' :active='trackedValues.tp_luma_pools_east' />
          <WotwTrackerTeleporterView style="grid-column: 4; grid-row: 2;" name='Glades' :active='trackedValues.tp_wellspring_glades' />
          <WotwTrackerTeleporterView style="grid-column: 6; grid-row: 2;" name='Willow' :active='trackedValues.tp_willow_inner' />
          <WotwTrackerTeleporterView style="grid-column: 8; grid-row: 2;" name='Outer Ruins' :active='trackedValues.tp_windtorn_ruins_outer' />
          <WotwTrackerTeleporterView style="grid-column: 3; grid-row: 3;" name='Marsh' :active='trackedValues.tp_inkwater_marsh' />
          <WotwTrackerTeleporterView style="grid-column: 4; grid-row: 3;" name='Hollow' :active='trackedValues.tp_kwoloks_hollow' />
          <WotwTrackerTeleporterView style="grid-column: 5; grid-row: 3;" name='Woods West' :active='trackedValues.tp_silent_woods_west' />
          <WotwTrackerTeleporterView style="grid-column: 6; grid-row: 3;" name='Woods East' :active='trackedValues.tp_silent_woods_wast' />
          <WotwTrackerTeleporterView style="grid-column: 7; grid-row: 3;" name='Wastes West' :active='trackedValues.tp_windswept_wastes_west' />
          <WotwTrackerTeleporterView style="grid-column: 8; grid-row: 3;" name='Wastes East' :active='trackedValues.tp_windswept_wastes_east' />
          <WotwTrackerTeleporterView style="grid-column: 3; grid-row: 4;" name='Burrows' :active='trackedValues.tp_midnight_burrows' />
          <WotwTrackerTeleporterView style="grid-column: 4; grid-row: 4;" name="Howl's Den" :active='trackedValues.tp_howls_den' />
          <WotwTrackerTeleporterView style="grid-column: 5; grid-row: 4;" name='Depths' :active='trackedValues.tp_mouldwood_depths' />
          <WotwTrackerTeleporterView style="grid-column: 8; grid-row: 4;" name='Inner Ruins' :active='trackedValues.tp_windtorn_ruins_inner' />
        </div>
        <div class='done-label'>
          <div ref='hype' class='hype'>
            <img src='@/assets/images/ori_hype.png'>
          </div>
        </div>
      </div>
      <div v-else-if='!hideConnectingScreen' key='connecting' class='fill-height d-flex flex-column align-center justify-center'>
        <div class='pb-4'>
          <template v-if='!receivedPacket'>
            Waiting for game...
          </template>
          <template v-else-if='connectedOnce'>
            Connection lost. Trying to reconnect...
          </template>
          <template v-else>
            Waiting for connection...
          </template>
        </div>
        <v-progress-circular indeterminate />
      </div>
    </v-fade-transition>
  </div>
</template>

<script>
  import { mapState } from 'vuex'
  import { decodePacket } from '~/assets/proto/ProtoUtil'
  import { ResetTracker, TrackerFlagsUpdate, TrackerUpdate } from '~/assets/proto/messages'
  import { applyTransparentWindowStyles, isOBS } from '~/assets/lib/obs'
  import { isElectron } from '~/assets/lib/isElectron'
  import { confettiFromElement } from '~/assets/lib/confettiFromElement'

  export default {
    name: 'Tracker',
    layout: 'plain',
    data: () => ({
      connected: false,
      receivedPacket: false,
      connectedOnce: false,
      hideConnectingScreen: false,
      trackedValues: {},
      seedFlags: [],
      showDone: false,
    }),
    head: {
      title: 'Item Tracker',
    },
    computed: {
      ...mapState('electron', ['settings', 'settingsLoaded']),
      trackerSource() {
        const source = this.$route.query.source

        if (!source) {
          return 'ws://127.0.0.1:31410'
        }

        if (/^wss?:\/\//.test(source)) {
          return source
        }

        return `${this.$paths.WS_BASE_URL}/remote-tracker/${source}`
      },
      isOBS,
      isElectron,
      showWillowHearts() {
        const showWillowHearts = this.$route.query.hearts === 'true' || this.settings?.LocalTracker?.ShowWillowHearts
        const hideHeartsUntilFirstOne = this.$route.query.hideHeartsUntilFirst === 'true' || this.settings?.LocalTracker?.HideHeartsUntilFirstHeart

        return showWillowHearts && (!hideHeartsUntilFirstOne || this.heartCount > 0)
      },
      showTeleporters() {
        return this.$route.query.teleporters === 'true'
      },
      showErrors() {
        return this.$route.query.errors === 'true'
      },
      heartCount() {
        const hearts = [
          'heart_wind_spinners',
          'heart_spinning_lasers',
          'heart_upper_heart',
          'heart_burrow_heart',
          'heart_willow_laser',
          'heart_redirect_puzzle',
          'heart_boulder_escape',
          'heart_lower_left',
        ]
        let count = 0

        for (const heart of hearts) {
          if (this.trackedValues[heart]) {
            count++
          }
        }

        return count
      },
    },
    watch: {
      connected: {
        immediate: true,
        handler(newValue) {
          if (!newValue && isOBS() && !this.showErrors) {
            setTimeout(() => {
              this.hideConnectingScreen = true
            }, 5000)
          } else if (newValue) {
            this.hideConnectingScreen = false
          }
        }
      },
      'trackedValues.game_finished'(value) {
        if (value) {
          setTimeout(() => {
            confettiFromElement(this.$refs.hype, {
              startVelocity: 30
            })
          }, 75)

          this.showDone = true
          setTimeout(() => {
            this.showDone = false
          }, 4000)
        }
      },
    },
    mounted() {
      if (isOBS() || isElectron()) {
        applyTransparentWindowStyles()

        // pagchimp
        document.documentElement.style.overflow = 'hidden'
      }

      this.connect()
    },
    beforeDestroy() {
      this.tryDisconnect()
    },
    methods: {
      connect() {
        if (!this.trackerSource) {
          return
        }

        console.log(`tracker: Trying to connect to ${this.trackerSource}...`)

        this.tryDisconnect()
        this.ws = new WebSocket(this.trackerSource)

        this.ws.addEventListener('close', () => {
          console.log(`tracker: Connection lost. Will retry in 2s...`)
          this.connected = false
          this.receivedPacket = false
          setTimeout(this.connect, 2000)
        })

        this.ws.addEventListener('open', () => {
          console.log(`tracker: Connected`)
          this.connected = true
          this.connectedOnce = true
        })

        this.ws.addEventListener('message', async event => {
          const packet = await decodePacket(event.data)

          if (!packet) {
            return
          }

          const handlePacket = () => {
            switch (packet.$type) {
              case TrackerUpdate.$type:
                this.$set(this.trackedValues, packet.id, packet.value)
                this.receivedPacket = true
                break
              case TrackerFlagsUpdate.$type:
                this.seedFlags = packet.flags
                this.receivedPacket = true
                break
              case ResetTracker.$type:
                this.trackedValues = {}
                this.seedFlags = []
                break
            }
          }

          if (this.$route.query.delay) {
            setTimeout(handlePacket, Number(this.$route.query.delay) * 1000)
          } else {
            handlePacket()
          }
        })
      },
      tryDisconnect() {
        try {
          this.ws?.close()
        } catch (e) {
          console.error(e)
        }
      },
    }
  }
</script>

<style lang='scss' scoped>
  .electron-draggable {
    -webkit-user-select: none;
    -webkit-app-region: drag;
  }

  .tracker-container {
    width: 100vw;
    position: relative;
    overflow: hidden;

    .done-label {
      position: absolute;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      display: flex;
      align-items: center;
      justify-content: center;
      flex-direction: column;

      .hype {
        height: 20vw;
        width: 20vw;
        overflow: hidden;
        display: flex;
        border-radius: 50%;
        border: 1vw solid var(--v-accent-base);
        margin-top: -5vw;
        transform: translateY(5vw) scale(1.1);
        opacity: 0;
        transition: opacity 100ms, transform 400ms cubic-bezier(.05, 1.62, .32, 1.01);

        img {
          width: 100%;
          height: 100%;
          border-radius: 50%;
        }
      }
    }

    &.done {
      > .tracker,
      > .teleporters {
        opacity: 0.4;
      }

      .label {
        animation: label-move both 400ms 400ms cubic-bezier(.05, 1.62, .32, 1.01);
      }

      .done-label {
        opacity: 1;
        transform: scale(1);
      }

      .hype {
        opacity: 1;
        transform: translateY(0) scale(1);
      }
    }
  }

  .tracker {
    display: grid;
    grid-template-columns: repeat(7, 1fr);
    width: 100%;
    height: calc(4 / 7 * 100vw);
    transition: opacity 200ms;

    > * {
      min-width: 0;
      min-height: 0;
    }

    .clean-water {
      grid-column: 5;
      grid-row: 1 / span 2;
    }

    .resource-view {
      grid-column: 6 / span 2;
      grid-row: 1 / span 2;
      position: relative;
    }
  }

  .teleporters {
    display: grid;
    grid-template-columns: repeat(8, 1fr);
    width: 100%;
    transition: opacity 200ms;

    > * {
      min-width: 0;
      min-height: 0;
    }
  }
</style>
