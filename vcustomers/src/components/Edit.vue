<template>
  <div class="update container">
      <Alert v-if="alert" v-bind:message="alert"></Alert>
    <h1 class="page-header">修改用户信息</h1>
    <form v-on:submit="updateCustomer">
        <div class="well">
            <h4>用于信息</h4>
            <div class="form-group">
                <label>姓名</label>
                <input type="text" class="form-control" placeholder="name" v-model="customer.name">
            </div>
            <div class="form-group">
                <label>电话</label>
                <input type="text" class="form-control" placeholder="phone" v-model="customer.phone">
            </div>
            <div class="form-group">
                <label>邮箱</label>
                <input type="text" class="form-control" placeholder="email" v-model="customer.email">
            </div>
            <div class="form-group">
                <label>学历</label>
                <input type="text" class="form-control" placeholder="education" v-model="customer.education">
            </div>
            <div class="form-group">
                <label>毕业学校</label>
                <input type="text" class="form-control" placeholder="graduationschool" v-model="customer.graduationschool">
            </div>
            <div class="form-group">
                <label>职业</label>
                <input type="text"  class="form-control" placeholder="profession" v-model="customer.profession">
            </div>
            <div class="form-group">
                <label>个人简介</label>
                <textarea calss="form-control" rows="10" cols="138" v-model="customer.profile"></textarea>
              
            </div>
            <button type="submit" class="btn btn-primary">修改</button>
        </div>
    </form>
  </div>
</template>

<script>
import Alert from "./Alert"
export default {
  name: 'update',
  data () {
    return {
        customer:{},
        alert:""
    }
  },
  methods: {
      fetchCustomer(id){
          this.$http.get("http://localhost:3001/users/"+id)
          .then((response) => {
            //   console.log(response);
            this.customer = response.data;
              
          })
      },
      updateCustomer(e){
        //   console.log(123);
        if(!this.customer.name || !this.customer.phone || !this.customer.email) {
            // console.log("请修改对应的信息");   
            this.alert = "请修改对应的信息";
        }else {
            let updateCustomer = {
                name:this.customer.name,
                phone:this.customer.phone,
                email:this.customer.email,
                education:this.customer.education,
                graduationschool:this.customer.graduationschool,
                profession:this.customer.profession,
                profile:this.customer.profile
            }
            // console.log(1233);
            
            this.$http.put("http://localhost:3001/users/"+this.$route.params.id,updateCustomer)
            .then((response) => {
                this.$router.push({path:"/",query:{alert:"用户信息更新成功"}});
                // alert("添加用户成功")
            })
             e.preventDefault();
        }
        // 阻止默认事件
          e.preventDefault();
      }
  },
  created(){
      this.fetchCustomer(this.$route.params.id);
  },
  components: {
      Alert
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
