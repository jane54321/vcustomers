<template>
  <div class="customers container">
      <Alert v-if="alert" v-bind:message="alert"></Alert>
    <h1 class="page-header">用户管理系统</h1>
    <input type="text" class="form-control" placeholder="搜索" v-model="filterInput">
    <br>
    <table class="table table-striped">
        <thead>
          <!-- 主页展示三列数据 -->
            <tr>
                <th>姓名</th>
                <th>电话</th>
                <th>邮箱</th>
                <th></th>
            </tr>
        </thead>
        <tbody>
          <!-- 这是从json:server 里面获取这些数据 -->
            <tr v-for="customer in filterBy(customers,filterInput)">
                <td>{{customer.name}}</td>
                <td>{{customer.phone}}</td>
                <td>{{customer.email}}</td>
                <td><router-link class=" btn btn-default" v-bind:to="'/customer/'+customer.id">详情</router-link></td>
            </tr>
        </tbody>
    </table>
  </div>
</template>


<script>
import Alert from './Alert'
export default {
  name: 'customers',
  data () {
    return {
        customers:[],
        alert: "",
        filterInput:""
    }
  },
  methods: {
      fetchCustomers(){
        
          this.$http.get("http://localhost:3001/users")
          // .then(function(response) {  // 这是es6的语法  
            .then((response) => {    //这个是使用axios  底下response.body  更换为 response.data
              // console.log(response);
            this.customers = response.data;          
          })  
      },
      filterBy(customers,value){
           //   遍历整个数组
          return customers.filter(function(customer){
            //   返回匹配到的对象
              return customer.name.match(value);
          })
      }      
  },
 
    created(){
      // 在router的目标组件上获取入参
        if(this.$route.query.alert){
            this.alert = this.$route.query.alert;
        }
      this.fetchCustomers();
  },
    updated(){
      this.fetchCustomers();
  },

    components: {
      Alert 
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style  scoped>

</style>
