<template>
  <div class="main">
  <div class="accountsStyle">
    <div>
        <button @click="getAccounts()">Get accounts</button>
            <h2>AccountList:</h2><br/>
            <ul id="example">
                <li v-for = "(item,index) in accounts">{{index}}: {{item}}</li>
            </ul>
<input type="text" name="inputAccountId" v-model="inputAccountId" placeholder="输入账户地址"/><br/>
            <button @click="getBalance()">Get Balance</button><h2>Balance rest: {{balance}}</h2><br/>
    </div>
    <div>
      <h3>创建账户</h3>
      <form @submit.prevent="crateAccount">
          <span>用户名：</span><input type="text" name="userName" v-model="userName" placeholder="用户名"/><br/>
          <span>地址：</span><input type="text" name="userAddress" v-model="userAddress" placeholder="地址"/><br/>
          <span>手机号码：</span><input type="text" name="userPhone" v-model="userPhone" placeholder="手机号码"/><br/>
          <span>学校：</span><input type="text" name="school" v-model="userSchool" placeholder="学校"/><br/>
          <span>账户名称：</span><input type="text" name="accountName" v-model="accountName" placeholder="账户名称"/><br/>
          <span>账户密码：</span><input type="password" name="accountPassword" v-model="accountPassword" placeholder="账户密码"/><br/>
         <input type="submit" value ="创建账号"/><br/> 
      </form>
      <h2>新建账户 ：{{newAccount}}</h2>
    </div>
    <div>
      <h3>账户信息查询</h3>
      <form @submit.prevent="getAccountsInfo">
        <span>账户地址</span><input type="text" name = "accountIdForInfo" v-model="accountIdForInfo" placeholder="账户地址"/><br/>
        <input type="submit" value ="信息查询"/><br/> 
      </form>
    </div>
   </div> 
   <div class="transtractionStyle">   
    <h2>转账</h2>
    <from @submit.prevent="sendTranstraction">
    <span>转出账户：</span><input type="text" name="fromAccountId" v-model="fromAccountId" placeholder="输入转出账户地址"/><br/>
    <span>账户密码：</span><input type="password" name="password" v-model="password" placeholder="输入转出账户密码"/><br/>
    <span>转入账户：</span><input type="text" name="toAccountId" v-model="toAccountId" placeholder="输入转入账户地址"/><br/>
    <span>交易备注：</span><input type="text" name="tardeInfo" v-model="tardeInfo" placeholder="输入交易备注"/><br/>
    <input type="submit" value="发送交易"/><br/>
    </from>
   </div>
  </div>
  
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      accounts:"",
      inputAccountId:"",
      balance:"",
      userName:'',
      userAddress:'',
      userPhone:'',
      userSchool:'',
      accountName:'',
      accountPassword:'',
      tardeInfo:'',
      toAccountId:'',
      password:'',
      fromAccountId:'',
      newAccount:'',
      accountIdForInfo:''
    }
  },
  methods:{
          getAccounts(){
            this.$axios({
              method:'post',
              url:'/api/getAllAccounts',
            }).then((res)=>{
              let data = res.data;
              this.accounts = data ;
            }).catch((error) =>{
              console.log(error);
            })
        },
        getBalance(){
          this.$axios({
            method:'post',
            url:'/api/getBalance',
            data: {
                accountId:this.inputAccountId,
               },
            }).then((res)=>{
                 this.balance = res.data
            }).catch((error) =>{
                  this.balance = "查询失败 :"+error
            })
        },
        sendTranstraction(){
           this.$axios({
             method:'post',
             url:'',
             data:{

             }
           }).then((res)=>{
             this.transtractionHash =res.data
           }).catch((error) =>{
             this.transtractionHash = "查询失败 ："+error
           }) 
        },
        crateAccount(){
          this.$axios({
            method:'post',
            url:'/api/createAccount',
            data:{
              userName:this.userName,
              address:this.userAddress,
              phone:this.userPhone,
              school:this.userSchool,
              accountName:this.accountName,
              password:this.accountPassword
            }
          }).then((res)=>{
            this.newAccount = res.data
          }).catch((error)=>{
            this.newAccount = "创建失败 ："+error
          })
        },
        getAccountsInfo(){
          this.$axios({
            method:'post',
            url:'/api/getAccountsInfo',
            data:{
                accountId:this.accountIdForInfo   
            }
          }).then((res)=>{
            console.log(res.data)
          }).catch((error)=>{
            console.log(error)
          })
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.main{
  float:left;
  border:1px ;
  width:100%;
}
.accountsStyle {
  float:left;
  border:1px;  
  width:40%;
}
.transtractionStyle{
  float:left;
  border:1px;  
  width:40%;
}
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
