<template>
  <div class="text-dark bg-light border-dark">[{{JSON.stringify(new Date()).replace("T"," ").replace('"',"").replace(/\..*?$/g,"")}}] 组件1-name:{{name}} </div>
</template>

<script lang='ts'>
import { Component, Vue, Prop, Emit, Watch  } from 'vue-property-decorator';

@Component
export default class First extends Vue {
    @Prop() private name!: string;
}

</script>

<style lang='scss' scoped>

</style>