<template>
  <div id="wrap">
    <div id="top_content">
      <div id="header">
        <div id="rightheader">
          <p>
            2009/11/20
            <br/>
          </p>
        </div>
        <div id="topheader">
          <h1 id="title">
            <a href="#">Main</a>
          </h1>
        </div>
        <div id="navigation">
        </div>
      </div>
      <div id="content">
        <p id="whereami">
        </p>
        <h1>
          update Emp info:
        </h1>
        <form action="emplist.html" method="post">
          <table cellpadding="0" cellspacing="0" border="0"
                 class="form_table">
            <tr>
              <td valign="middle" align="right">
                id:
              </td>
              <td valign="middle" align="left">
                {{emp_id}}
              </td>
            </tr>
            <tr>
              <td valign="middle" align="right">
                name:
              </td>
              <td valign="middle" align="left">
                <input type="text" class="inputgri" name="name" v-model="emp_name"/>
              </td>
            </tr>
            <tr>
              <td valign="middle" align="right">
                photo:
              </td>
              <td valign="middle" align="left">
                <img :src="emp_img" width="50" height="50">
                <input type="file" name="img" ref="img"/>
              </td>
            </tr>
            <tr>
              <td valign="middle" align="right">
                salary:
              </td>
              <td valign="middle" align="left">
                <input type="text" class="inputgri" name="salary" v-model="emp_salary"/>
              </td>
            </tr>
            <tr>
              <td valign="middle" align="right">
                age:
              </td>
              <td valign="middle" align="left">
                <input type="text" class="inputgri" name="age" v-model="emp_age"/>
              </td>
            </tr>
          </table>
          <p>
            <el-button @click="updata"></el-button>
          </p>
        </form>
      </div>
    </div>
    <div id="footer">
      <div id="footer_bg">
        ABC@126.com
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Update",
  data(){
    return {
      emp_list:[],
      emp_name:'',
      emp_id:'',
      emp_age:'',
      emp_img:'',
      emp_salary:'',
    }
  },
  methods:{
    getEmp(id){
      this.$axios({
        url:"http://127.0.0.1:8000/ems_app/emp/"+id+"",
        method: "get",
      }).then(response=>{
        let data = response.data.results;
        this.emp_id = data.id;
        this.emp_name = data.emp_name;
        this.emp_age = data.age;
        this.emp_img = data.img;
        this.emp_salary = data.salary;
      })
    },
    created(){
      let emp_id = this.$route.params.id;
      this.getEmp(emp_id);
    },
    updata(){
      let emp_file = this.$refs.img.files[0];
      let formData = new FormData();
      formData.append("emp_name", this.emp_name);
      formData.append("salary", this.emp_salary);
      formData.append("age", this.emp_age);
      formData.append("img", emp_file);
      console.log(formData);
      this.$axios({
        url:"http://127.0.0.1:8000/ems_app/emp/"+this.emp_id+"/",
        method:"patch",
        data:formData,
        headers: {
          "content-type": "multipart/form-data"
        },
      }).then(response => {
        console.log(response.data);
        this.emp_list = response.data.results;
        if (response.data.message) {
          this.$message({
            message: '恭喜你，更新成功',
            type: 'success',
            duration: 1000,
            showClose: true,
          });
          // 跳转到列表页
          this.$router.push("/index");
        }
      }).catch(error => {
        console.log(error.message);
      })
    }
    }

}
</script>

<style scoped>

</style>
