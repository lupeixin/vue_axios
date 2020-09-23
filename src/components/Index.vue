<template>
  <div id="wrap">
    <div id="top_content">
      <div id="header">
        <div id="rightheader">
          <p>
            2009/11/20
            <br/>
            安全退出
          </p>
        </div>
        <div id="topheader">
          <h1 id="title">
            <a href="#">main</a>
          </h1>
        </div>
        <div id="navigation">
        </div>
      </div>
      <div id="content">
        <p id="whereami">
        </p>
        <h1>
          欢迎，{{ user_msg }}, 访问百知员工管理系统
        </h1>
        <table class="table">
          <tr class="table_header">
            <td>ID</td>
            <td>Name</td>
            <td>Photo</td>
            <td>Salary</td>
            <td>Age</td>
            <td>Operation</td>
          </tr>
          <tr class="row1" v-for="(emp,index) in emp_list" :key="emp.id">
            <td>{{ emp.id }}</td>
            <td>{{ emp.emp_name }}</td>
            <td><img :src="emp.img" style="height: 60px;"></td>
            <!--                        <td>{{emp.img}}</td>-->
            <td>{{ emp.salary }}</td>
            <td>{{ emp.age }}</td>
            <td><a href="javascript:;" @click="del_emp(emp.id)">删除员工</a>&nbsp;
              <router-link :to="'/update/'+emp.id">更新员工</router-link>
            </td>
          </tr>

        </table>
        <p>
          <!--                    <input type="button" class="el-button" value="添加员工"/>-->
          <el-button type="success">
            <router-link to="/add">添加员工</router-link>
          </el-button>
        </p>
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
  name: "Index",
  data() {
    return {
      user_msg: "",
      emp_list: [],
    }
  },
  methods: {
    // 获取所有用户信息
    findALLEmp() {
      this.$axios.get("http://127.0.0.1:8000/ems_app/emp/").then(response => {
        this.emp_list = response.data.results;
      }).catch(error => {

      })
    },
    // 删除员工
    del_emp(id) {
      // 根据id删除员工
      this.$axios({
        url: "http://127.0.0.1:8000/ems_app/emp/"+id+"/",
        method: "delete",
      }).then(response => {
        if (response.data.message) {
          this.$message({
            message: '恭喜你，删除成功',
            type: 'success',
            duration: 1000,
            showClose: true,
          })
        } else {
          this.$message.error("删除失败")
        }
      }).catch(error => {
        this.$message.error("删除失败")
      })
    }
  },
  // vue的生命周期钩子
  created() {
    let username = sessionStorage.getItem("user");
    if (username) {
      this.user_msg = username;
    } else {
      this.$message.error("对不起，请登录");
      this.$router.push("/login");
    }
    // 在页面加载完成前获取员工数据并把值赋值给 data
    this.findALLEmp();
  }
}
</script>

<style scoped>

</style>
