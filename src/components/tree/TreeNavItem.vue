<template>
    <span :class="['nav-item',{'selected':this.model.navShow}]"  @click.stop="handleClick" >{{model.name}}</span>
</template>
<script>
    import { findComponentUpward } from '../../utils/utils';
    import Emitter from '../../mixins/emitter';
    const transitionTime = 300; // from CSS
    export default {
        name:"navItem",
        props: {
            model:{
                type: Object,
                default () {
                    return {};
                }
            },
        },
        data() {
            return {

            }
        },
        created(){
            this.treeSelect = findComponentUpward(this, 'treeSelect');
        },
        methods: {
            open(){
                this.$forceUpdate();
                this.$set(this.model,'navShow',true);
            },
            close(){
                this.$forceUpdate();
                this.$set(this.model,'navShow',false);
            },
            handleClick() {
                //防止过快点击
                if (this.treeSelect.transitioning) return;
                this.treeSelect.transitioning = true;
                setTimeout(() => this.treeSelect.transitioning = false, transitionTime);
                this.treeSelect.closeOtherNavItem(this);
                this.open();
                this.treeSelect.getActiveNavItemIndex(this);
            }
        }

    }
</script>  
    