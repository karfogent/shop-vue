<template>
  <div class="relative">
    <div class="h-full" @mouseenter="toggle()" @mouseleave="toggle()">
      <slot />
    </div>
    <div v-show="isShown" class="classes" :class="classes">{{text}}</div>
  </div>
</template>

<script>
export default {
  name: "toolTip",
  props: {
    text: {
      type: String
    }
  },
  data(){
    return{
      isShown: false
    }
  },
  computed: {
    classes() {
      return [
        'top-10',
        'left-1/2',
      ]
    }
  },
  methods: {
    toggle(){
      this.isShown = !this.isShown
    }
  }
}
</script>

<style scoped lang="scss">
.classes{
  background-color: #2D2D2D;
  opacity: 80%;
  color: white;
  padding: 12px 18px;
  white-space: nowrap;
  font-size: 12px;
  position: absolute;
  &::after{
    content: " ";
    position: absolute;
    bottom: 100%;
    left: 10%;
    margin-left: -5px;
    border-width: 5px;
    border-style: solid;
    border-color: transparent transparent #2D2D2D transparent;
  }
}
</style>