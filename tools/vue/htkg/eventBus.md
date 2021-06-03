> 类似与vuex但是代码可读性低

1. 在 src 新建 emit/emit.js，.js文件内容：

```
import Vue from 'vue';

var Emit = new Vue({});

export {
	Emit
}

```

2. 在 src/main.js 下引入

```
import { Emit } from './emit/emit.js'

Vue.prototype.Emit = Emit;
```

3. 在mounted生命周期里通过this.Emit.$on()监听。在destroyed生命周期里面通过this.Emit.$off()解除绑定。一定要解除绑定事件.并且on所在的页面的生成一定在emit之前

```
//父组件：index.vue：

	<div class="indexPageWrap">
		<header></header>
        <footer></footer>
	</div>


   import header from './header.vue';
   import footer from './footer.vue';
	export default{
		data () {
			return {
				index:0
			}
		},
		mounted () {
			this.Emit.$on('fromHeader',this.indexFormHeader);
            this.Emit.$on('fromFooter',this.indexFromFooter);
		},
        //注意：在组件销毁时，一定要解除绑定事件：
        destroyed(){
                this.Emit.$off('fromHeader');
                this.Emit.$off('fromFooter');
        },
		components : {
			header
		},
		methods : {
			indexFormHeader(vlaue){
        console.log('from header');
          console.log(value)
      },
      indexFromFooter(){
        console.log('from footer')
      }
		}
	}

```

- 通过this.Emit.$emit()传递数据

```
//子组件 header.vue

	<div class="headerPageWrap">
		<div @click="headerEmit">Emit<div>
	</div>

<script type="text/javascript">
	export default{
		data () {
			return {
				index:0
			}
		},
		mounted () {
			this.Emit.$on('headerTofooter',this.headerFromFoot)
		},
		components : {
			
		},
		methods : {
			headerEmit(){
            	this.Emit.$emit('fromHeader',{value:"123"}); //可以传递数据
            },
            headerFromFoot(){
            	console.log('from footer');
            }
		}
	}
</script>
<style type="text/css">
	
</style>

```