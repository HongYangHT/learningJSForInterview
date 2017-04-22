### vuex 的相关学习
#### 构造器选项
- state  
  - vuex 使用单一状态树，用一个对象包含所有应用层级状态，并将其作为一个唯一的数据源。（每个应用只有一个store实例）
  
  ```javascript
  vue.use(vuex);
  const store = new Vuex.Store({
  	state:{
  		...
  	}
  	...
  });

  const app = new Vue({
  	store
  });
  ```
- getters
  - vuex 允许使用getter（相当于state的计算属性）  
  
  ```javascript
  const store = new Vuex.Store({
  	state:{
  		...
  	},
  	getters:{
  		countGetter: (state,getters) => { return state.count; }
  	}
  })

  const app = new Vue({
  	store,
  	computed:{
  		doneTodosCount: () => {
		    return this.$store.getters.countGetter
		}
		...mapGetters([
			'countGetter'
			...
		]) // 需要注意这里是数组

		...mapGetters([{
			countGetter:'countGetter' // 别名
		}{
			...
		}])
  	}
  });
  ```

- mutations
  - 更改 Vuex 的 store 中的状态的唯一方法是提交 mutation，Vuex 中的 mutations 非常类似于事件：每个 mutation 都有一个字符串的 事件类型 (type) 和 一个 回调函数 (handler)，这个回调函数就是我们实际进行状态更改的地方，并且它会接受 state 作为第一个参数

  ```javascript
  const store = new Vuex.Store({
	  state: {
	    count: 1
	  },
	  mutations: {
	    increment :(state, ...rest) => {
	      // 变更状态
	      state.count++
	      console.log(rest);
	    }
	  }
	})
	
  ```

  - 你不能直接调用一个 mutation handler。这个选项更像是事件注册：“当触发一个类型为 increment 的 mutation 时，调用此函数。”要唤醒一个 mutation handler，你需要以相应的 type 调用 store.commit 方法：

  ```javascript
  const app = new Vue({
  	store,
  	methods:{
  	  setIncrement (){
  	  	this.$store.commit('increment');
  	  	this.$store.commit({type:'increment',a:1})
  	  }
  	}
  });

  ```

- actions
  - Action 提交的是 mutation，而不是直接变更状态。Action 可以包含任意异步操作。  
  Action 函数接受一个与 store 实例具有相同方法和属性的 context 对象，因此你可以调用 context.commit 提交一个 mutation，或者通过 context.state 和 context.getters 来获取 state 和 getters  

  ```javascript
  const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count++
    }
  },
  actions: {
    increment (context) {
      context.commit('increment')
    }
  }
})
  ```

  - 分发action，目的在于模块间的通信
  - 组合actions 使用方式是利用promise或者是async/await来进行同步操作


- modules
  - Vuex 允许我们将 store 分割到模块（module）每个模块拥有自己的 state、mutation、action、getters、甚至是嵌套子模块——从上至下进行类似的分割
  
  ```javascript
	  const moduleA = {
	  state: { ... },
	  mutations: { ... },
	  actions: { ... },
	  getters: { ... }
	}

	const moduleB = {
	  state: { ... },
	  mutations: { ... },
	  actions: { ... }
	}

	const store = new Vuex.Store({
	  modules: {
	    a: moduleA,
	    b: moduleB
	  }
	})

	store.state.a // -> moduleA 的状态
	store.state.b // -> moduleB 的状态
  ```

- plugins
- strict

#### 辅助函数
- mapState  
  * 当一个组件需要获取多个状态时候，将这些状态都声明为计算属性会有些重复和冗余。为了解决这个问题，我们可以使用 mapState 辅助函数帮助我们生成计算属性

  ```javascript
  import {mapState} from 'vuex';
  const app = new Vue({
  	store,
  	computed:mapState({
  		count: state => state.count,
  		...
  	});
  });
  ```
  * mapState 展开运算符

  ```javascript
  import {mapState} from 'vuex';
  const app = new Vue({
  	store,
  	computed:{
  		<!-- other:function(){}, -->
  		other () => {},
  		...mapState({
  			...
  		})
  	}
  });
  ```
- mapGetters
  - 如上getters
- mapMutations
- mapActions

********************************************************************
mutations and actions 学习

mutations 是通过commit来触发action，且mutations是同步执行的
actions 提交的是mutations，而不是直接变更状态，并且actions是可以异步的
actions 是通过context参数来获取state and getters的（一个与 store 实例具有相同方法和属性的 context 对象）

actions可以通过dispatch进行分发，eg: store.dispatch('increment')

也就是说，利用mutations进行数据的改变，而利用actions进行数据的请求等异步操作

getters getters相当于vuex的计算属性，新增的属性

其中mapXXX 为vuex的辅助函数，需要在使用前进行引用

********************************************************************