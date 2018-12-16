<template>
<div class="row">
    <div class="card col-4 border-dark" style="width: 18rem;">
        <h3 class="card-header bg-transparent text-success">使用说明</h3>
        <div class="card-body">
            <ul class="list-group">
                <li class="list-group-item">1.动态组件实现tab切换效果</li>
                <li class="list-group-item">2.并添加组件的功能</li>
            </ul>
            <div class="card-text">
                <div>1.动态切换区</div>
                <textarea name="" id="" class="col" rows="10">动态地绑定到它的 is 特性，我们让多个组件可以使用同一个挂载点，并动态切换。如果把切换出去的组件保留在内存中，可以保留它的状态或避免重新渲染。为此可以添加一个 keep-alive 指令参数.通过切换后的时间可以看到keep-alive的作用
                </textarea>
            </div>
            <div class="card-text">
                <div>2.动态添加组件</div>
                <textarea name="" id="" class="col" rows="10">
                    利用vue中的v-for语法糖特性，通过监听当前组件内部的组件名列表对象。当触发按钮（添加组件名）的时候，在列表中添加指定的名字就会动态添加组件
                </textarea>
            </div>
        </div>
    </div>
    <div class="col card bg-dark">
        <div class="card-header">
            <ul class="nav nav-pills card-header-pills">
                <li class="nav-item"><a class="nav-link" :class="currentView===first?'active':''" href="javascript:void(0);" @click="toggleTabs(first);">{{first}}</a></li>
                <li class="nav-item"><a class="nav-link" :class="currentView===second?'active':''" href="javascript:void(0);" @click="toggleTabs(second);">{{second}}</a></li>
                <li class="nav-item"><a class="nav-link" :class="currentView===third?'active':''" href="javascript:void(0);" @click="toggleTabs(third);">{{third}}</a></li>
            </ul>
            <ul class="nav nav-pills card-header-pills">
                <li class="nav-item"><a class="nav-link" :class="currentView===third?'active':''" href="javascript:void(0);" @click="pop();">删除最后一个元素{{componetslist[componetslist.length-1]}}</a></li>
            </ul>
        </div>
        <div class="card-body bg-light">
            <h6 class="text-primary">1.切换区</h6>
            <keep-alive>
            <div :is="currentView" :name="currentView"></div>
            </keep-alive>
        </div>
        <div class="card-body bg-primary">
            <h6 class="text-light">2.组件动态添加区</h6>
            <anytags ref="index" v-for="(value,index) in componetslist" :is="value" :key="index" :name="value" keep-alive>
            </anytags>
        </div>
        <div class="card-body bg-primary" id="manual2">
            <h6 class="text-light">3.组件动态manual2删除区</h6>

        </div>
        <div class="card-body bg-primary" id="manual">
            <h6 class="text-light">3.组件动态manual1删除区</h6>
        </div>

    </div>
</div>
</template>

<script lang='ts'>
import { Component, Vue, Prop, Emit, Watch } from 'vue-property-decorator';
import First from '@/testDynamic/First.vue';
import Second from '@/testDynamic/Second.vue';
import Third from '@/testDynamic/Third.vue';
@Component({
    components: {
        first: First,
        Second,
        Third
    },
})
export default class Dynamic extends Vue {

    @Prop({default: false }) public initshow!: boolean;
    private name = 111;
    private currentView = "first";
    private first = "first"; // 导航栏文本1
    private second = "Second"; // 导航栏文本2
    private third = "Third";
    private deletecomponents: Set<any> = new Set();
    private componetslist: string[] = ["first","Second", "Third"];
    @Emit()
    private toggleTabs(tabText: string) {
        this.currentView = tabText;
        this.componetslist.push(tabText);
    }
    @Emit()
    private pop() {
        const totallen =  (this.$refs.index as any).length-1;
        // tslint:disable-next-line:variable-name
        let delete_uid: number = 0;
        let deleteVue = null;
        for (let i = totallen;i >=0 ;i--) {
            console.log("111");
            const nowid = (this.$refs.index as any)[i]._uid;
            if(!this.deletecomponents.has(nowid)) {
                delete_uid = nowid;
                deleteVue = (this.$refs.index as any)[i];
                break;
            }
        }
        // tslint:disable-next-line:no-unused-expression
        // 更新uid
        this.deletecomponents.add(delete_uid);
        console.log(this.deletecomponents,delete_uid);
        // 页面有两个id 只能挂载到最后append的元素之上
        (document.getElementById('manual') as any).appendChild(deleteVue.$el as any);
        (document.getElementById('manual2') as any).appendChild(deleteVue.$el as any);
        // tslint:disable-next-line:no-unused-expression
        // this.deletecomponents.push(this.componetslist.pop());
    }
}
</script>

<style lang='scss' scoped>
.card-body{
    flex: none;
}
</style>