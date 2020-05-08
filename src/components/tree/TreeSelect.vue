<template>
    <div class="tree-wrapper">
        <div class="tree-header">
            <div class="head-title">{{title}}</div>
            <Scroll class="head-nav" :scrollX="true"  :data="navList">
                <div class="nav-content">
                      <TreeNavItem  v-for="(item,index) in navList" :key="index"  :model="item" ref="navItem"></TreeNavItem>
                      <span class="nav-item nav-empty" v-if="navList.length===0">{{placeholder}}</span>
                </div>
            </Scroll>
        </div>
        <div class="tree-content" :style="contentStyle"
             @touchstart.prevent="tabsTouchStart"
             @touchmove.prevent="tabsTouchMove"
             @touchend="tabsTouchEnd">
             <TreeMenu v-for="(item,index) in menuList" :key="index" :model="item" ref="menu"></TreeMenu>
        </div>
    </div>
</template>
<script>
    import TreeMenuItem from './TreeMenuItem.vue';
    import TreeMenu from './TreeMenu.vue';
    import TreeNavItem from'./TreeNavItem.vue';
    import Emitter from '../../mixins/emitter';
    import Scroll from '../scroll/Scroll.vue'
    import { findComponentsDownward } from '../../utils/utils';
    const moveSize = 80;
    export default {
        name:"treeSelect",
        mixins: [ Emitter],
        components:{
            TreeMenuItem,
            TreeMenu,
            TreeNavItem,
            Scroll
        },
        props:{
            data: {
                type: Array,
                default () {
                    return [];
                }
            },
            title:{
                type: String,
                default () {
                    return "all Menus";
                }
            },
            placeholder:{
                type: String,
                default () {
                    return "请选择";
                }
            }
        },
        data() {
            return {
                navList:[],
                menuList:this.data,
                activeIndex:0,
            }
        },
        computed: {
            contentStyle () {
                const x = this.activeIndex;
                const p = x === 0 ? '0%' : `-${x*50}%`;
                let style = {};
                if (x > -1) {
                    style = {
                        transform: `translateX(${p}) translateZ(0px)`
                    };
                }
                return style;
            },
        },
        watch:{
            data(value){
                this.menuList = value;
            },
        },
        created(){
            this.touch = {}
        },
        mounted(){
            this.initHandle();
        },
        methods:{
            tabsTouchStart(e){
                this.touch.initiated = true
                // 用来判断是否是一次移动
                this.touch.moved = false
                const touch = e.touches[0]
                this.touch.startX = touch.pageX
                this.touch.startY = touch.pageY
            },
            tabsTouchMove(e){
                if (!this.touch.initiated) {
                    return
                }
                const touch = e.touches[0]
                const deltaX = touch.pageX - this.touch.startX
                const deltaY = touch.pageY - this.touch.startY
                if (Math.abs(deltaY) > Math.abs(deltaX)) {
                    return
                }
                if (!this.touch.moved) {
                    this.touch.moved = true
                }
                this.touch.size = Math.abs(deltaX);
                this.touch.direction = deltaX>0?-1:1
            },
            tabsTouchEnd(){
                if (!this.touch.moved) {
                    return
                }
                if(this.touch.size>moveSize){
                    if(this.touch.direction>0){
                        this.goNext();
                    }else {
                        this.goPrev();
                    }
                }
                this.touch.initiated = false
            },
            goNext(){
                if(this.activeIndex === this.navList.length-1){
                    this.activeIndex = 0;
                }else {
                    this.activeIndex = this.activeIndex +1;
                }
                this.activeNavStyleByIndex(this.activeIndex)
            },
            goPrev(){
                if(this.activeIndex===0){
                    this.activeIndex = this.navList.length-1;
                }else {
                    this.activeIndex = this.activeIndex -1;
                }
                this.activeNavStyleByIndex(this.activeIndex)
            },
            activeNavStyleByIndex(index){
                let navItems = this.$refs.navItem;
                this.closeOtherNavItem(navItems[index]);
                navItems[index].open();
            },
            //获取点击navItem索引
            getActiveNavItemIndex(navItem){
                let  navItems = this.$refs.navItem;
                let index = navItems.findIndex(item => item===navItem);
                this.activeIndex = index;
                return this.activeIndex;
            },
            closeOtherNavItem(navItem){
                let  navItems = this.$refs.navItem;
                navItems.forEach((item,index)=>{
                    if(item!==navItem){
                        item.close();
                    }
                })
            },
            renderMenu(menu,menuItem){
                this.removeMenuTo(menu);
                this.addMenu(menuItem.model);
                this.closeMenuItemTo(menu);
                this.renderNav(menuItem);
            },
            removeMenuTo(menu){
                while (this.menuList.length>1){
                    if(this.menuList[this.menuList.length-1]===menu.model){
                        break;
                    }else {
                        this.removeMenu();
                    }
                }
            },
            renderNav(menuItem){
                if(this.navList.length===0){
                    this.addNav(menuItem.model);
                    this.activeLastNav();
                    return;
                }
               //判断点击的是第几个menu
                let index = this.getMenuIndex(menuItem);
                this.activeIndex = index;
                //判断是否是当前的item
                if(this.navList[index]===menuItem.model){
                    this.navList = this.navList.slice(0,index+1);
                }else {
                    this.navList = this.navList.slice(0,index);
                    this.addNav(menuItem.model);
                }
                this.activeLastNav();
            },
            activeLastNav(){
               for(let i = 0;i<this.navList.length;i++){
                   if(i===this.navList.length-1){
                       this.$set(this.navList[i],'navShow',true);
                   }else {
                       this.$set(this.navList[i],'navShow',false);
                   }
               }
            },
            //处理其他的menu
            closeMenuItemTo(menu){
                let menus = this.$refs.menu;
                for(let i = menus.length-1;i>0;i--){
                    if(menu ===menus[i]){
                        break;
                    }else {
                        let menuItems = findComponentsDownward(menus[i], 'menuItem');
                        menuItems.forEach((item,index)=>{
                            item.close();
                        })
                    }
                }
            },
            //获取当前menu的序号
            getMenuIndex(menuItem){
                const menu = menuItem.menu;
                let menus = this.$refs.menu;
                return menus.findIndex(item=>item ===menu)
            },
            removeNav(){
                this.navList.pop();
            },
            removeMenu(){
                this.menuList.pop();
            },
            addNav(model){
                this.navList.push(model);
            },
            addMenu(model){
                this.menuList.push(model);
            },
            //点击导航按钮
            clickNavItem(navItem){
                this.$emit('on-clickNavItem',navItem);
            },
            //点击菜单栏按钮
            clickMenuItem(model,menuItem){
                this.$emit('on-clickMenuItem',model,menuItem);
            },
            renderLeaf(model,menuItem){
                this.$emit('on-renderLeaf',model,menuItem);
            },
            //派发事件
            initHandle(){
                this.$on('clickNavItem',this.clickNavItem);
                this.$on('clickMenuItem',this.clickMenuItem);
                this.$on('renderLeaf',this.renderLeaf);
            }
        }

    }
</script>
<style scoped>
    @import "./tree-select.css";
</style>
    