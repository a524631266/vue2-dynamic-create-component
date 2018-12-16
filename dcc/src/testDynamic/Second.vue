<template>
  <div class="text-light bg-dark border-danger candrag" draggable="true">
    <div>[{{JSON.stringify(new Date()).replace("T"," ").replace('"',"").replace(/\..*?$/g,"")}}] 组件2</div>
    <p>props属性name值:<span style="color:white;background:blue">[{{name}}]</span>
    </p>
  </div>
</template>

<script lang='ts'>
import { Component, Vue, Prop, Emit, Watch  } from 'vue-property-decorator';

@Component
export default class Second extends Vue {
    @Prop() private name!: string;
    // private name = "嘿嘿";
}
</script>

<style lang='scss' scoped>

</style>