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
                    使用vue.extend 继承扩展构造函数来动态创建子组件，本应用没有进行任何扩展，所以，相当于是原组件的一份class copy,
                    同时在父组件中维护用的子组件，并在子组件的props的相关变量做监听，同时使用全局函数set方法，同步更新数据
                </textarea>
            </div>
        </div>
    </div>
    <div class="col card bg-dark">
        <div class="card-header">
            <div>
                <ul class="nav nav-pills card-header-pills">
                    <li class="nav-item"><a class="nav-link" :class="currentView===first?'active':''" href="javascript:void(0);" @click="addComponent(first);">{{first}}</a></li>
                    <li class="nav-item"><a class="nav-link" :class="currentView===second?'active':''" href="javascript:void(0);" @click="addComponent(second);">{{second}}</a></li>
                    <li class="nav-item"><a class="nav-link" :class="currentView===third?'active':''" href="javascript:void(0);" @click="addComponent(third);">{{third}}</a></li>
                </ul>
            </div>
            <ul class="nav nav-pills card-header-pills">
                <li class="nav-item"><a class="nav-link" :class="currentView===third?'active':''" href="javascript:void(0);" @click="pop();">删除最后一个元素</a></li>
            </ul>
            <div class="input-group">
                <input class="input-group-append form-control" type="text" v-model="ownnames[first]" placeholder="firstname"/>
                <input class=" form-control" type="text" v-model="ownnames[second]" placeholder="secondname"/>
                <input class="input-group-append form-control" type="text" v-model="ownnames[third]" placeholder="thirdname"/>
            </div>
        </div>
        <div class="card-body bg-light">
            <h6 class="text-primary">1.切换区</h6>
            <keep-alive>
            <component :is="currentView" :name="ownnames[currentView]"></component>
            </keep-alive>
        </div>
        <div class="card-body bg-primary" >
            <h6 class="text-light">2.组件动态添加区</h6>
            <div id="addarea"></div>
        </div>
        <div class="card-body bg-primary" id="manual2">
            <h6 class="text-light">3.组件动态manual2删除区</h6>
        </div>
        <button @click="changename">change</button>
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
    private componetsinstancelist = {[this.first]: First,[this.second]: Second,[this.third]: Third};
    private ownnames = {[this.first]: "name1",[this.second]: "name2",[this.third]: "name3"};
    private childrenVue: {[x: string]: Vue[]} = {[this.first]: [],[this.second]: [],[this.third]: []};
    public mounted() {
        const firstConstruct = Vue.extend(First);
        // tslint:disable-next-line:new-parens
        const firstinst =  (new firstConstruct({
        })).$mount();
        this.$set(firstinst,"name",this.ownnames[this.first]);
        // Vue.set(firstinst,"name",this.ownnames[this.first]);
        (document.getElementById("addarea") as any).appendChild(firstinst.$el as any);
        this.childrenVue[this.first].push(firstinst);
    }
    @Emit()
    private changename() {
        this.name = 444;
    }
    @Emit()
    private addComponent(tabText: string) {
        this.currentView = tabText;
        const construct = Vue.extend(this.componetsinstancelist[tabText]);
        // tslint:disable-next-line:new-parens
        const instacne = (new construct({
        })).$mount();
        Vue.set(instacne,"name",this.ownnames[tabText]);
        (document.getElementById("addarea") as any).appendChild(instacne.$el as any);
        this.childrenVue[tabText].push(instacne);
    }
    @Emit()
    // tslint:disable-next-line:no-empty
    private pop() {
        const vuelist = this.childrenVue[this.currentView];
        const lastvue = vuelist[vuelist.length-1];
        (document.getElementById('manual2') as any).appendChild(lastvue.$el as any);
    }
    @Watch("ownnames",{deep: true })
    private updateChildren(newVal: {[x: string]: string}, oldVal: {[x: string]: string}) {
        // Vue.set(this.childrenVue[0],"name","222222");
        Object.keys(oldVal).forEach(
            (value) => {
                // console.log(oldVal[value],newVal[value]);
                this.childrenVue[value].forEach(
                    (vueinstance) => {
                        Vue.set(vueinstance,"name",newVal[value]);
                    }
                );
            }
        );
    }
}
</script>

<style lang='scss' scoped>
.card-body{
    flex: none;
}
</style>