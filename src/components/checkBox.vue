<template>
  <div v-if="this.$props.checkBox" >
    <svg @click="checkHandler()" width="16" height="16" viewBox="0 0 16 16" fill="none" xmlns="http://www.w3.org/2000/svg" class="checkBox">
      <rect x="0.5" y="0.5" width="15" height="15" fill="#FCE66F" stroke="#C4C4C4"/>
      <path d="M3 7.5L7.28571 11L13 4" stroke="black"/>
    </svg>
  </div>

  <div v-else>
    <svg @click="checkHandler()" width="16" height="16" viewBox="0 0 16 16" fill="none" xmlns="http://www.w3.org/2000/svg" class="checkBox">
      <rect x="0.5" y="0.5" width="15" height="15" stroke="#C4C4C4"/>
    </svg>
  </div>
</template>

<script>
export default {
  name: "checkBox",
  props: {
    checkBox: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return{
      checkBoxValue: false
    }
  },
  methods: {
    checkHandler() {
     this.$props.checkBox = this.$props.checkBox !== true
    }
  }
}
</script>

<style scoped>
.checkBox{
  cursor: pointer;
}
</style>