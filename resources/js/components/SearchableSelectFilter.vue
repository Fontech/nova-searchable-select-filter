<template>
  <div>
    <h3 class="text-sm uppercase tracking-wide text-80 bg-30 p-3">
      {{ filter.name }}
    </h3>

    <div class="p-2">
      <search-input
        :dusk="`${filter.name}-searchable-filter-select`"
        :value="value"
        :data="filteredOptions"
        :track-by="trackBy"
        ref="searchInput"
        @input="handleInput"
        @clear="handleClear"
        @selected="handleSelected"
      >
        <template name="default">
          <div class="text-80" v-if="filter.currentValue">{{ filter.currentValue }}</div>
          <div class="text-70" v-else>{{ __('Click to choose') }}</div>
        </template>
        <template slot="option" slot-scope="{ option }">
          {{ option[trackBy] }}
        </template>
      </search-input>
    </div>
  </div>
</template>

<script>
import _ from 'lodash'

export default {
  props: {
    resourceName: {
      type: String,
      required: true,
    },
    filterKey: {
      type: String,
      required: true,
    },
    lens: String,
  },

  data: () => ({
    trackBy: 'name',
    search: null
  }),

  methods: {
    handleInput(search) {
      this.search = search
    },
    handleClear() {
      this.search = null
      this.updateFilterState(null)
    },
    handleSelected(selected) {
      this.updateFilterState(selected.value)
    },
    updateFilterState(value) {
      this.$store.commit(`${this.resourceName}/updateFilterState`, {
        filterClass: this.filterKey,
        value,
      })

      this.$emit('change')
    }
  },

  computed: {
    filter() {
      return this.$store.getters[`${this.resourceName}/getFilter`](this.filterKey)
    },

    filteredOptions() {
      const options = this.filter.options

      if (!this.search) {
        return options
      }

      return _.filter(options, option => option[this.trackBy].includes(this.search))
    },

    value() {
      return _.find(this.filter.options, ['value', this.filter.currentValue])
    },
  },
}
</script>
