# tree-select

![Image text](https://github.com/haomashroom/tree-select/blob/master/data/readme.jpg)

## 介绍

### 多层级动态展开菜单（适用于手册、树形结构、多层级选项卡、目录导航等等）

### 初始化
```
       created(){
            var res = {};
            res.name = "全部";
            res.href = "";
            res.children =[{name:"test"},{name:"test1"},{name:"test2"},{name:"test3"},{name:"test"},{name:"test1"},{name:"test2"},{name:"test3"},{name:"test4"},{name:"test5"},{name:"test6"},{"isDefault":1,"src":"images/icontype1.png","children":[{name:"A模块",children:[{name:'AA模块',children:[{name:"AAA模块"}]},{name:'AA模块',children:[{name:"AAc模块"}]},{name:'Ab模块',children:[{name:"Abb模块"}]}]},{name:"B模块",children:[{name:'BB模块',children:[{name:"BBB模块"}]}]}],"domain":"PB07","maptype":"CONTAINER","name":"维修公告","oid":"com.imecms.container.Container:1001"},{"path":"用户手册","children":[{name:"DDDDDD模块",children:[{name:'AA模块',children:[{name:"AAA模块"}]},{name:'CCCCC模块',children:[{name:"AAc模块"}]},{name:'Ab模块',children:[{name:"Abb模块"}]}]},{name:"B模块",children:[{name:'BB模块',children:[{name:"BBB模块"}]}]}],"src":"images/icontype1.png","domain":"PB07","maptype":"ST","name":"用户手册","oid":"com.imecms.container.Folder:3837"},{"path":"保养手册","src":"images/icontype1.png","domain":"PB07","maptype":"ST","name":"保养手册","oid":"com.imecms.container.Folder:3838"},{"path":"下载区","src":"images/icontype1.png","domain":"PB07","maptype":"JS","name":"下载区","js":"showDownloadAreaContent();"}];
            this.list = [res];
        },
        methods:{
            //点击导航按钮
            clickNavItem(navItem){
                console.log("navItem1",navItem);
            },
            //点击菜单节点
            clickMenuItem(model,menuItem){
                console.log("model111",model);
                console.log("model22",menuItem);
            },
            //点击最后一个叶子节点
            renderLeaf(model,menuItem){
                var data1 = [{name:"新数据大可",children:[{name:"测试数据1"}]},{name:"新数据2",children:[{name:"测试数据2"}]}];
                menuItem.addSubItems(data1);
            },

        }
```
## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Run your tests
```
npm run test
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
