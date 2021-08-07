<template>
  <div id="recruiter-register">
    <div class="page-header">
      <div class="container">
        <div class="row-form">
          <div class="col-lg-12">
            <div class="inner-header">
              <h3>{{ isCreated ? "Cập nhật" : "Tạo mới" }} công ty</h3>
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
                <label class="labels">Tên công ty</label>
                <input type="text" class="form-control" v-model="name" />
              </div>
            </div>
            <div class="row mt-3">
              <div class="col-md-12">
                <label class="labels">Trụ sở chính</label>
                <input type="text" class="form-control" v-model="headquarters" />
              </div>
            </div>

            <div class="row mt-3">
              <div class="col-md-12">
                <label class="labels">Website công ty</label>
                <input type="text" class="form-control" v-model="website" />
              </div>
            </div>            
            <div class="row mt-3">
              <div class="col-md-12">
                <label class="labels">Mô tả công ty</label>
                <input type="text" class="form-control" v-model="description" />
              </div>
            </div>
           
            <div class="mt-5 text-center">
                <button
                  class="btn btn-common log-btn"
                  type="btn btn-common log-btn"
                  @click.prevent="updateCompany"
                >
                  {{ isCreated ? "Cập nhật" : "Tạo mới" }}
                </button>
            </div>
          </div>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      name: "",
      token: "",
      headquarters: "",
      website: '',
      description: '',
      error: false,
      isCreated: false
    };
  },
  methods: {
    updateCompany() {
      const response = this.isCreated ? axios.put('http://capstone2021-test.ap-southeast-1.elasticbeanstalk.com/recruiter/company/update', {
            name: this.name,
            headquaters: this.headquarters,
            website: this.website,
            description: this.description,
          }, {
            headers: {
              Authorization: `Bearer ${localStorage.getItem("token")}`,
            },
          }) :
      axios.post('http://capstone2021-test.ap-southeast-1.elasticbeanstalk.com/recruiter/company/create', {
            name: this.name,
            headquaters: this.headquarters,
            website: this.website,
            description: this.description,
        }, {
          headers: {
            Authorization: `Bearer ${localStorage.getItem("token")}`,
          },
      });
      response.catch((error) => {
        console.log(error.response);
        if (error.response.status == 400) {
          this.error = true;
        }
      }).then((res) => {
        console.log(res);
        this.$router.push('/recruiter-profile');
      })


    },
  },

  mounted() {
    if (localStorage.getItem("token")) {
      this.token = localStorage.getItem("token");
    }

    const header = {
      "Content-Type": "application/json",
      Authorization: `Bearer ${this.token}`,
    };
    axios.get("http://capstone2021-test.ap-southeast-1.elasticbeanstalk.com/recruiter/company/self",
        {
          headers: header
        }
    ).then((response) => {
      if (response.status === 200) {
        this.isCreated = true;
      }
      this.name = response.data.data.name;
      this.headquarters = response.data.data.headquaters;
      this.website = response.data.data.website;
      this.description = response.data.data.description;
    }).catch((e) => {
      if (e.response.status === 404) {
        this.isCreated = false;
      }
    })
  },
};
</script>

<style>
</style>