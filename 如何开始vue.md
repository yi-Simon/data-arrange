1. 模板

```
<template>
  <div></div>
</template>
<script>
import xxx from xxx
export default{
  name,
  data,
  components
}
</script>
<style lang="stylus" scoped></style>
```

2. vuex
   
   ```
   import {api} from api.js
   import {mutationType} from mutaionType.js
   Vue.use(Vuex)
   const state = {}
   const mutations = {}
   const actions = {}
   const getters = {}
   const modules = {}
   export default new Vuex.Store {
     state,mutations,actions,getters,mudules:{}
   }
   ```

3. router

```
重写路由原型push和replace
const router = new VueRouter({
  routes,scrollBehavior(to,from,savedPosition){
    return {x:0, y:0}
  }
})
export default router
```
