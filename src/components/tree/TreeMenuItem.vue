<template>
    <li @click.stop="handleClick" :class="['tree-item',{'selected':this.model.open}]"><a href="javascript:void(0)" class="menu-link">{{model.name}}</a></li>
</template>
<script>
    import { findComponentUpward } from '../../utils/utils';
    import Emitter from '../../mixins/emitter';
    export default {
        name:'menuItem',
        mixins: [ Emitter],
        props: {
            model:{
                type: Object,
                default () {
                    return {};
                }
            },
        },
        created(){
            this.menu = findComponentUpward(this, 'treeMenu');
        },
        data() {
            return {}
        },
        methods: {
            open(){
                this.$forceUpdate();
                this.$set(this.model,'open',true);
            },
            close(){
                this.$forceUpdate();
                this.$set(this.model,'open',false);
            },
            toggle(){
                this.$forceUpdate();
                this.$set(this.model,'open',!this.model.open);
            },
            addSubItems(array){
                this.model.children = array;
            },
            handleClick() {
                //判断是否是叶子节点
                if(!this.model.children||this.model.children.length===0){
                    this.menu.treeSelect.renderLeaf(this.model,this);
                }
                this.menu.closeOtherItem(this);
                this.open();
                this.menu.treeSelect.renderMenu(this.menu,this);
                this.menu.treeSelect.clickMenuItem(this.model,this);
            }
        }
    }
</script>  
    