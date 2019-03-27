<style lang='scss'>
.base_input{
    color: red;
}
</style>
<template>
 <div class="base_input">
     <input type="text" :value="value" @input="$emit('input',$event.target.value)">
 </div>
</template>

<script>
export default {
  name: 'baseInput',
  model: {
    prop: 'value',
    event: 'input'
  },
  //   prop: {
  //     type: String,
  //     default: 'text'
  //   },
  data () {
    return {

    }
  },
  methods: {},
  created () {}
}
</script>
