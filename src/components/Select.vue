<template>
  <div class="formBody">
    <select class="formSelect" :name="selectId" :id="selectId" @change="$emit('changeValue', $event.target)" v-model="defaultValue"> 
      <option value="">Select value</option>
      <option v-for="(opt, index) in optionValues[selectId]" :key="index" :value="opt">{{opt}}</option>
    </select>
  </div>
</template>

<script>
import { toRef } from '@vue/reactivity'
export default {
name:'select-view',
props:{
  selectId: {
    type: String
  },
  optionValues: {
    type: Object
  },
  selectedParam: {
    type: String
  }
},
setup(props){
  const defaultValue = toRef(props, 'selectedParam')
  return {
    defaultValue
  }
}
}
</script>

<style scoped>
.formBody{
    display: flex;
    justify-content: center;
    width: 50%;
  }
.formSelect{
  padding: 5px 10px;
  margin-bottom: 10px;
  width: 100%;
}
</style>