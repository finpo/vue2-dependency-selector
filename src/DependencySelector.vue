<template lang="pug">
div
  select(v-for="(item, index) in selectors", v-model="insideValue[item]", @change="cleanDependencySelected(index)", :class="Array.isArray(classes) ? classes[index] : classes || ''")
    option(value="") {{ emptyhint ? ( Array.isArray(emptyhint) ? emptyhint[index] : emptyhint) : 'choose' }}
    option(v-for="option in getOptionsArray(item)") {{option[item]}}
</template>

<script>
/*
eslint no-console: ["error", { allow: ["warn", "error"] }]
*/
import { uniqBy, filter, find, cloneDeep, pick, isEqual, forEach, zipObject } from 'lodash';

export default {
  props: {
    value: {
      type: Object,
    },
    selectors: {
      type: Array,
      required: true,
    },
    source: {
      type: Array,
      required: true,
    },
    emptyhint: {
      type: [Array, String],
    },
    classes: {
      type: [Array, String],
    },
  },
  data() {
    return {
      insideValue: zipObject(this.selectors, this.selectors.map(() => '')),
    };
  },
  created() {
    // break init if don't passed v-model.
    if (typeof this.value === 'undefined') return;
    // dependency break check.
    const check = this.selectors.map(key => this.value[key]);
    try {
      check.forEach((value, index) => {
        if ((typeof value === 'undefined' || value === '') && typeof check[index + 1] !== 'undefined') {
          throw new Error(`[DependencySelector] Dependency Breaking...
            seems look like your assign object value break dependency
            you can't escape parent selector , assign child value ...
            we will not assign data you provide.`.replace(/ {12}/gm, ''));
        }
      });
    } catch (e) {
      console.error(e);
      this.emitOut();
      return;
    }
    // if provide data can find only one row , just assign data
    if (filter(this.source, this.value).length === 1) {
      this.insideValue = pick(find(this.source, this.value), this.selectors);
    } else {
      // assign value passed
      forEach(pick(this.value, this.selectors), (value, key) => {
        this.insideValue[key] = value || '';
      });
    }
  },
  watch: {
    insideValue: {
      handler: 'emitOut',
      deep: true,
    },
    value: 'applyValueToInside',
  },
  methods: {
    applyValueToInside() {
      const inside = pick(this.insideValue, this.selectors);
      const outside = pick(this.value, this.selectors);
      if (!isEqual(inside, outside)) {
        this.insideValue = outside;
      }
    },
    cleanDependencySelected(index) {
      this.selectors.slice(index + 1).forEach((key) => {
        this.insideValue[key] = '';
      });
    },
    getOptionsArray(key) {
      const currentIndex = this.selectors.indexOf(key);
      const parentKey = this.selectors[currentIndex - 1];
      const query = currentIndex ? { [parentKey]: this.insideValue[parentKey] } : {};
      return uniqBy(filter(this.source, query), key);
    },
    emitOut() {
      this.$emit('input', find(this.source, this.insideValue) ? find(this.source, this.insideValue) : cloneDeep(this.insideValue));
    },
  },
};
</script>
