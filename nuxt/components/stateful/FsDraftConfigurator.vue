<template>
  <div class="m800">
    <template>
      <h3>{{ $t('patternOptions') }}</h3>
      <v-expansion-panel>
        <v-expansion-panel-content v-for="(group, index) in $fs.conf.patterns[pattern].optiongroups" :key="index">
          <div slot="header">
            <h4>{{ index }}
            <v-badge overlap color="accent" v-if="$store.state.draft.custom[index]">
              <span slot="badge" class="fs-custom-count">{{ $store.state.draft.custom[index]}}</span>
              &nbsp;
            </v-badge>
            </h4>
          </div>
          <v-card>
            <v-card-text>
              <v-expansion-panel>
                <template v-for="option in group">
                  <fs-option-radio
                    v-if="options[option].type === 'chooseOne'"
                    :key="option"
                    :pattern="pattern"
                    :name="option"
                    :option="options[option]"
                    :dflt="$store.state.draft.defaults.patternOptions[option]"
                  />
                  <fs-option-slider
                    v-else
                    :key="option"
                    :pattern="pattern"
                    :name="option"
                    :option="options[option]"
                    :dflt="(options[option].type === 'measure')
                    ? $fs.asMm($store.state.draft.defaults.patternOptions[option], $store.state.user.account.units)
                    : $store.state.draft.defaults.patternOptions[option]
                    "
                  />
                </template>
              </v-expansion-panel>
            </v-card-text>
          </v-card>
        </v-expansion-panel-content>
      </v-expansion-panel>

      <h3>{{ $t('draftOptions') }}</h3>
      <v-expansion-panel>
        <fs-option-sa />
        <fs-option-scope />
        <fs-option-theme />
      </v-expansion-panel>
    </template>
  </div>
</template>

<script>
import FsOptionSlider from '~/components/stateful/FsOptionSlider'
import FsOptionRadio from '~/components/stateful/FsOptionRadio'
import FsOptionSa from '~/components/stateful/FsOptionSa'
import FsOptionScope from '~/components/stateful/FsOptionScope'
import FsOptionTheme from '~/components/stateful/FsOptionTheme'

export default {
  name: 'FsDraftConfigurator',
  components: {
    FsOptionSlider,
    FsOptionRadio,
    FsOptionSa,
    FsOptionScope,
    FsOptionTheme,
  },
  props: {
    pattern: {
      type: String,
      required: true
    },
    model: {
      type: String,
      required: true
    },
  },
  data: function() {
    return { options: this.$fs.conf.patterns[this.pattern].options }
  },
  computed: {
    customCount () {
      return this.$store.state.draft.custom
    }
  }
}
</script>
