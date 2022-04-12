
 <template>
 <div   class='container' >

  <el-header>Header</el-header>
  <el-container stye="display:flex">
    <el-aside width="200px">
<el-menu
      default-active="2"
      class="el-menu-vertical-demo"
      @open="handleOpen"
      @close="handleClose"
      background-color="#545c64"
      text-color="#fff"
      active-text-color="#ffd04b"  router>
      <el-submenu :index="item.id+''"  v-for="item in List1" :key="item.id">
        <template slot="title">
          <i class="el-icon-location"></i>
          <span>{{item.value}}</span>
        </template>
        <el-menu-item index="2">
        <i class="el-icon-menu"></i>
        <span slot="title">导航二</span>
      </el-menu-item>
        
      </el-submenu>
      
    </el-menu>



    </el-aside>
    <el-main>
     
     <el-form :model="ruleForm" status-icon :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
  <el-form-item label="密码" prop="pass">
    <el-input type="password" v-model="ruleForm.pass" autocomplete="off"></el-input>
  </el-form-item>
  <el-form-item label="确认密码" prop="checkPass">
    <el-input type="password" v-model="ruleForm.checkPass" autocomplete="off"></el-input>
  </el-form-item>
  <el-form-item label="年龄" prop="age">
    <el-input v-model.number="ruleForm.age"></el-input>
  </el-form-item>
  <el-form-item>
    <el-button type="primary" @click="submitForm('ruleForm')">提交</el-button>
    <el-button @click="resetForm('ruleForm')">重置</el-button>
    <el-button type="info" @click="logout">推出</el-button>
     <el-button type="info" @click="faqingqiu">获取数据</el-button>
  </el-form-item>
</el-form>
 </el-main>
  </el-container>

 </div>
 </template>
 
 <script>
 export default {
data() {
  
      var checkAge = (rule, value, callback) => {
        if (!value) {
          return callback(new Error('年龄不能为空'));
        }
        setTimeout(() => {
          if (!Number.isInteger(value)) {
            callback(new Error('请输入数字值'));
          } else {
            if (value ===18) {
              callback(new Error('必须年满18岁'));
            } else {
              callback();
            }
          }
        }, 1000);
      };
      var validatePass = (rule, value, callback) => {
        if (value === '') {
          callback(new Error('请输入密码'));
        } else {
          if (this.ruleForm.checkPass !== '') {
            this.$refs.ruleForm.validateField('checkPass');
          }
          callback();
        }
      };
      var validatePass2 = (rule, value, callback) => {
        if (value === '') {
          callback(new Error('请再次输入密码'));
        } else if (value !== this.ruleForm.pass) {
          callback(new Error('两次输入密码不一致!'));
        } else {
          callback();
        }
      };
      return {
        ruleForm: {
          pass: '',
          checkPass: '',
          age: ''
        },
        List1:[
  {
    id:1,
    value:'wxhn',
    children:[
    ]
  },
  {
    id:1,
    value:'wxhn2',
    children:[
    ]
  },
  {
    id:3,
    value:'wxhn3',
    children:[
    ]
  }
  
  ],
        rules: {
          pass: [
            { validator: validatePass, trigger: 'blur' }
          ],
          checkPass: [
            { validator: validatePass2, trigger: 'blur' }
          ],
          age: [
            { validator: checkAge, trigger: 'blur' }
          ]
        }
      };
    },
    methods: {
      submitForm(formName) {
        this.$refs[formName].validate((valid) => {
          if (valid) {
            alert('submit!');
          } else {
            console.log('error submit!!');
            return false;
          }
        });
      },
      resetForm(formName) {
        this.$refs[formName].resetFields();
      },
      logout(){
        this.$router.push("./advert");
       },
       http(page) {
      this.$api
        .getGoodsList({
          page,
        })
        .then((res) => {
          console.log(res.data);
          if (res.data.status === 200) {
            this.tableData = res.data.data; //数据列表
            this.total = res.data.total;
            this.pageSize = res.data.pageSize;
          }
        });
    },
      
      },
      created() {
    this.http(1);
  },

 
 }
 </script>
 
 <style>

   .container{
     height: 100%;

   }
.el-container{
  height: 100%;
  display: flex;
  
}

  .el-header {
    background-color: #B3C0D1;
    
    line-height: 60px;
  }
  
  .el-aside {
    background-color: #D3DCE6;
    height:140px ;
   
  }
  
  .el-main {
    background-color: #E9EEF3;
    color: pink;
    width:100%;
  }
  
 
  
   </style>