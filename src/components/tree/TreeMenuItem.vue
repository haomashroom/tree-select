<template>
    <li @click.stop="handleClick" :class="['tree-item',{'selected':this.model.open}]"><a href="javascript:void(0)" class="ui-link">{{model.name}}</a></li>
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
            handleClick() {
                this.menu.closeOtherItem(this);
                this.toggle();
                this.menu.treeSelect.renderMenu(this.menu,this);
            }
        }
    }
</script>  
    