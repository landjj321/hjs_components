<style lang='scss'>
.base_input{
    color: red;
}
</style>
<template>
 <div class="base_input">
     <input type="checkbox" :checked="checked" @input="$emit('change',$event.target.checked)">
 </div>
</template>

<script>
export default {
  name: 'baseCheckbox',
  model: {
    prop: 'checked',
    event: 'change'
  },
  props: {
    checked: Boolean
  },
  data () {
    return {

    }
  },
  methods: {},
  created () {}
}
</script>
