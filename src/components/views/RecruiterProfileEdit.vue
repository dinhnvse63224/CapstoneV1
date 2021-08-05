<template>
  <div id="recruiter-register">
    <div class="page-header">
      <div class="container">
        <div class="row-form">
          <div class="col-lg-12">
            <div class="inner-header">
              <h3>Nhà tuyển Dụng</h3>
            </div>
          </div>
        </div>
      </div>
    </div>
    <form @submit.prevent="recruiterProfileEdit">
      <div class="row">
        <div class="col-md-3 border-right">
          <div
            class="d-flex flex-column align-items-center text-center p-3 py-5"
          >
            <img
              class="rounded-circle mt-5"
              width="150px"
              src="https://st3.depositphotos.com/15648834/17930/v/600/depositphotos_179308454-stock-illustration-unknown-person-silhouette-glasses-profile.jpg"
            /><span class="font-weight-bold"></span>
            <span class="text-black-50"></span><span> </span>
          </div>
        </div>
        <div class="col-md-5 border-right">
          <div class="p-3 py-5">
            <div class="row mt-3">
              <div class="col-md-12">
                <label class="labels">Tên tài khoản</label>
                <input type="text" class="form-control" v-model="username" />
              </div>
            </div>
            <div class="row mt-3">
              <div class="col-md-12">
                <label class="labels">Mật khẩu</label>
                <input type="password" class="form-control" v-model="password" />
              </div>
            </div>

            <div class="row mt-3">
              <div class="col-md-12">
                <label class="labels">Nhập lại mật khẩu</label>
                <input type="password" class="form-control" v-model="confirmPassword" />
              </div>
            </div>            
            <div class="row mt-3">
              <div class="col-md-7">
                <label class="labels">Email</label>
                <input type="text" class="form-control" v-model="gmail" />
              </div>
              <div class="col-md-5">
                <label class="labels">Điện thoại</label>
                <input type="text" class="form-control" v-model="phone" />
              </div>
            </div>
            <div class="row mt-2">
              <div class="col-md-12">
                <label class="labels">Họ và tên</label>
                <input type="text" class="form-control" v-model="fullName" />
              </div>
            </div>
            <div v-if="error" class="row mt-3">
              <div class="col-md-12">
                <label class="error-message text-danger">Vui lòng điền đủ các thông tin.</label>
              </div>
            </div>
           
            <div class="mt-5 text-center">
                <button
                  class="btn btn-common log-btn"
                  type="btn btn-common log-btn"
                >
                  Cập nhật
                </button>
            </div>
          </div>
        </div>
<!--        <div class="col-md-4">-->
<!--          <div class="p-3 py-5">-->
<!--            <div class="row mt-3">-->
<!--              <div class="col-md-8">-->
<!--                <label class="labels" for="isCompany">Bạn là</label>-->
<!--                <select-->
<!--                  name="poc"-->
<!--                  id="isCompany"-->
<!--                  class="form-control"-->
<!--                  v-model="selected"-->
<!--                >-->
<!--                  <option value="personal">Nhà tuyển dụng tư nhân</option>-->
<!--                  <option value="company">Nhà tuyển dụng doanh nghiệp</option>-->
<!--                </select>-->
<!--              </div>-->
<!--            </div>-->
<!--            <div v-if="selected === 'company'" id="company">-->
<!--              <div class="row mt-2">-->
<!--                <div class="col-md-8">-->
<!--                  <label class="labels">Tên công ty</label>-->
<!--                  <input type="text" class="form-control" value="" />-->
<!--                </div>-->
<!--              </div>-->
<!--              <div class="row mt-3">-->
<!--                <div class="col-md-12">-->
<!--                  <label class="labels">Địa chỉ công ty</label>-->
<!--                  <input type="text" class="form-control" value="" />-->
<!--                </div>-->
<!--              </div>-->
<!--              <div class="row mt-3">-->
<!--                <div class="col-md-12">-->
<!--                  <label class="labels">Website của công ty</label>-->
<!--                  <input type="text" class="form-control" value="" />-->
<!--                </div>-->
<!--              </div>-->
<!--            </div>-->
<!--            <div v-else></div>-->
<!--          </div>-->
<!--        </div>-->
      </div>
    </form>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      profile: "",
      token: "",
      selected: "",     
      username: '',
      password: '',
      confirmPassword: '', 
      gmail: '',
      phone: '',
      avatar: '',
      fullName: '',
      error: false,
    };
  },
  methods: {
    async recruiterProfileEdit() {
      await axios.put('http://capstone2021-test.ap-southeast-1.elasticbeanstalk.com/recruiter/update', {
            fullName: this.fullName,
            gmail: this.gmail,
            phone: this.phone,
            username: this.username,
        }, {
          headers: {
            Authorization: `Bearer ${localStorage.getItem("token")}`,
          },

      }).catch((error) => {
        if (error.response.status == 400) {
          this.error = true;
        }
      }).then((res) => {
        localStorage.setItem(
            "recruiterProfile",
            res.config.data
        )

        this.$router.push('/recruiter-profile');
      })


    },
  },

  mounted() {
    if (localStorage.getItem("recruiterProfile")) {
      this.fullName = JSON.parse(localStorage.getItem("recruiterProfile")).fullName;
      this.gmail = JSON.parse(localStorage.getItem("recruiterProfile")).gmail;
      this.phone = JSON.parse(localStorage.getItem("recruiterProfile")).phone;
      this.username = JSON.parse(localStorage.getItem("recruiterProfile")).username;
    }
    if (localStorage.getItem("token")) {
      this.token = localStorage.getItem("token");
    }
  },
};
</script>

<style>
</style>