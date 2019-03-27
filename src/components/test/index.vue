<style>
    .test{
        font-size: 20px;
    }
</style>
<template>
 <div :class="'test'">
    <h1>这个是test的组件</h1>
    <input v-bind="$attrs" :value="value" @input="$emit('input',$event.target.value)">

 </div>
</template>

<script>
export default {
  components: {},
  inheritAttrs: false,
  props: ['label', 'value'],
  data () {
    return {

    }
  },
  methods: {},
  created () {
    console.log(this.$attrs)
  }
}
</script>
