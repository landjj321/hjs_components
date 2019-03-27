<style>
  .page1{
    padding-left: 30px;
  }
  .red{
    color: red;
  }
</style>
<template>
 <div class="page1">
     page1
    <button @click="up()">up</button>
    <button @click="down()">down</button>
    <actionsheet :actions="actions" ref="action"></actionsheet>
    <test data-id="aaa" class="red" placeholder="enter you username" required v-model='username'></test>

    <base-input v-model="val"></base-input>
    <p>{{val}}</p>

    <base-checkbox v-model="checked"></base-checkbox>
    <p>{{checked}}</p>

 </div>
</template>

<script>
// eslint-disable-next-line standard/object-curly-even-spacing
import {actionsheet, test, baseInput, baseCheckbox } from '../components'
export default {
  components: {
    actionsheet,
    test,
    baseInput,
    baseCheckbox
  },
  data () {
    return {
      actions: [
        {name: 'aaa', path: '/'},
        {name: 'bbb', path: '/'},
        {name: 'ccc', path: '/'},
        {name: 'ddd', path: '/'}
      ],
      username: '',
      val: '',
      checked: true

    }
  },
  methods: {
    up () {
      this.$refs.action.up()
    },
    down () {
      this.$refs.action.down()
    }

  },
  created () {}
}
</script>
