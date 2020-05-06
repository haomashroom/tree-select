<template>
   <Scroll class="tree-menu" :data="model.children">
       <ul class="tree-box">
           <TreeMenuItem v-for="(item,index) in model.children" :model="item" :key="index" ref="menuItem"></TreeMenuItem>
           <!--<li @click.stop="handleClick" :class="['tree-item',{'selected':this.model.open}]"><a href="javascript:void(0)" class="ui-link">{{model.name}}</a></li>-->
       </ul>
   </Scroll>
</template>
<script>
    import TreeMenuItem from './TreeMenuItem.vue'
    import { findComponentUpward } from '../../utils/utils';
    import Emitter from '../../mixins/emitter';
    import Scroll from '../scroll/Scroll.vue'
    export default {
        name:"treeMenu",
        mixins: [ Emitter],
        components:{
            TreeMenuItem,
            Scroll
        },
        props: {
            model:{
                type: Object,
                default () {
                    return {};
                }
            },
        },
        created(){
            this.treeSelect = findComponentUpward(this, 'treeSelect');
        },
        data() {
            return {}
        },
        methods:{
            closeOtherItem(MenuItem){
                let  menuItems = this.$refs.menuItem;
                menuItems.forEach((item,index)=>{
                    if(item!==MenuItem){
                        item.close();
                    }
                })
            }
        }

    }
</script>  
    