<template>
  <div class="text-dark bg-light border-dark candrag" draggable="true">
    <div>[{{JSON.stringify(new Date()).replace("T"," ").replace('"',"").replace(/\..*?$/g,"")}}] 组件1</div>
    <p>props属性name值:<span style="color:white;background:blue">[{{name}}]</span><br/>
    </p>
  </div>
</template>

<script lang='ts'>
import { Component, Vue, Prop, Emit, Watch, Model  } from 'vue-property-decorator';

@Component
export default class First extends Vue {
    @Prop() public name2!: string;
    @Prop() private name!: string;
    private mounted() {
      console.log("加载First");
    }
}

</script>

<style lang='scss'>
.candrag{
  cursor:move;
}
</style>



