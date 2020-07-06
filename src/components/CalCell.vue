<template>
  <div @click = 'onCellClick' :class="'calcell' + ` ${currentClass}`">
    {{data}}
  </div>
</template>

<script>
export default {
  name: 'calcell',
  props: {
    data: [String, Number],
  },
  data() {
    return {
      currentClass: null,
    }
  },
  methods: {
    onCellClick: function() {
      if (parseInt(this.data)) {
        this.$emit('on-cell-click', this.data)
      }
    }
  }
}
</script>

<style scoped>
.calcell {
  display: block;
  cursor: pointer;
  vertical-align: middle;
  text-align: center;
  float: left;
  margin: 4px;
  height: 50px;
  line-height: 50px;
  vertical-align: middle;
  width: 12.5%;
  z-index: 1;
}
.active {
  background-color: green;
}
.calcell:hover {
  background-color: yellow;
}
</style>
