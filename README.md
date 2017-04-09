# vue2-dependency-selector

> vue.js 2 component for multiple selector with dependency .
> you can use this make something like city area choose , or anything you want user interactive with dependencies .

## how to install
npm
```bash
npm install @finpo/vue2-dependency-selector
```
yarn
```bash
yarn add @finpo/vue2-dependency-selector
```


## how to use
```vue
<template>
  <section>
    <pre>{{selected}}</pre>
    <dependency-selector 
      v-model="selected" 
      :source="source" 
      :selectors="['city','area']" 
      :emptyhint="['Which city you live','Which area you live']" 
      :classes="['select-1','select-2']" 
    ></dependency-selector>
    <br/>
    <button @click="selected = { city: 'New Taipei City', area: 'Banqiao Dist.', zip: '220' }">assign data from outside</button>
  </section>
</template>

<script>
import DependencySelector from '@finpo/vue2-dependency-selector';

export default {
  components: {
    DependencySelector,
  },
  data() {
    return {
      selected: { zip: '103' },
      source: [
        { city: 'Taipei City', area: 'Zhongzheng Dist.', zip: '100' },
        { city: 'Taipei City', area: 'Datong Dist.', zip: '103' },
        { city: 'Taipei City', area: 'Zhongshan Dist.', zip: '104' },
        { city: 'Taipei City', area: 'Songshan Dist.', zip: '105' },
        { city: 'New Taipei City', area: 'Banqiao Dist.', zip: '220' },
        { city: 'New Taipei City', area: 'Xizhi Dist.', zip: '221' },
        { city: 'New Taipei City', area: 'Shenkeng Dist.', zip: '222' },
        { city: 'New Taipei City', area: 'Shiding Dist.', zip: '2223' },
      ],
    };
  },
};
</script>

```

## props
prop | required | type | desc
---- | -------- | ---- | ----
v-model |       | Object | data you want collect , you can pass data for default select . you don't need pass all of data .
source | true | Array (Object in Array) | your selector options data , one record one row .
selectors | true | Array (String in Array) | your selector will use this array let user choose .
emptyhint |      | String , Array (String Array) | option empty value text .
classes   |      | String , Array (String Array) | you can defined class name you want for selector ( everyone or each ) .

## online demo
[demo](https://vue2-dependency-selector.surge.sh/)

