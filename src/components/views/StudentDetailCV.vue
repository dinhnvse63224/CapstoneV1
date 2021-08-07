<template>
  <div id="student-profile">
    <div class="page-header">
      <div class="container">
        <div class="row-form">
          <div class="col-lg-12">
            <div class="inner-header">
              <h3>Thông tin chi tiết CV</h3>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="container border-bottom bg-white mt-1 pt-md-3 pt-2">
      <div class="d-flex flex-md-row justify-content-around align-items-center">
        <div class="d-flex flex-md-row align-items-center">
          <div class="p-md-2">
            <!-- <img
              src="https://st3.depositphotos.com/15648834/17930/v/600/depositphotos_179308454-stock-illustration-unknown-person-silhouette-glasses-profile.jpg"
              alt=""
              class="rounded-circle"
              id="profile"
            /> -->
          </div>
          <div class="p-md-2 p-1" id="info">
            <h5></h5>
            <div class="text-muted"></div>
          </div>
        </div>
      </div>
      <div class="pl-lg-5 pt-lg-2 pt-md-1">
        <div class="container rounded-bottom bg-light">
          <div class="row pl-lg-5 pt-md-0 pt-sm-2">
            <div class="col-md-6 py-md-4">
              <div class="d-flex flex-row">
                <div class="btn border-primary text-primary selected-tab">
                  <router-link :to="{path:'create-cv', query:{id:id}}">
                    Cập nhật thông tin CV</router-link
                  >
                </div>
              </div>
              <div class="d-flex flex-row">
                <div class="btn border-danger text-danger delete-tab">
                  <a href="" @click.prevent="deleteCV">Xoá CV</a>
                </div>
              </div>
              <div class="left">
                <div class="row mt-2">
                  <div class="col-md-12">
                    <label class="labels"
                    >Tên CV: {{ studentCv.cvName }}</label
                    >
                  </div>
                  <div class="col-md-12">
                    <label class="labels"
                    >Họ & tên: {{ studentCv.name }}</label
                    >
                  </div>
                  <div class="col-md-12">
                    <label class="labels">Giới tính: {{ studentCv.sex ? "Nam" : "Nữ" }}</label>
                  </div>
                  <div class="col-md-12">
                    <label class="labels">Ngày sinh: {{ studentCv.dob }}</label>
                  </div>
                  <div class="col-md-12">
                    <label class="labels">Trường học: {{ studentCv.school }}</label>
                  </div>
                  <div class="col-md-12">
                    <label class="labels">Kinh nghiệm: {{ studentCv.experience }}</label>
                  </div>
                  <div class="col-md-12">
                    <label class="labels">Ngoại ngữ: {{ studentCv.foreignLanguage }}</label>
                  </div>
                  <div class="col-md-12">
                    <label class="labels">Mức lương mong muốn: {{ studentCv.desiredSalaryMinimum }} VNĐ</label>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- job suggest -->
    <section id="featured" class="section bg-gray pb-45">
      <!-- <h2>Việc làm dành cho bạn</h2> -->
      <div class="container">
        <div class="row">
          <!-- <div class="col-lg-6 col-md-12 col-xs-12">
                    <a class="job-listings-featured" href="job-details.html">
                        <div class="row">
                            <div class="col-lg-6 col-md-6 col-xs-12">
                                <div class="job-company-logo">
                                    <img src="assets/img/features/img1.png" alt="">
                                </div>
                                <div class="job-details">
                                    <h3>Software Engineer</h3>
                                    <span class="company-neme">FPT Software</span>
                                    <div class="tags">
                                        <span><i class="lni-map-marker"></i> Ho Chi Minh</span>

                                    </div>
                                </div>
                            </div>
                            <div class="col-lg-6 col-md-6 col-xs-12 text-right">
                                <div class="tag-type">
                                    <sapn class="heart-icon">
                                        <i class="lni-heart"></i>
                                    </sapn>
                                    <span class="part-time">Part Time</span>
                                </div>
                            </div>
                        </div>
                    </a>
                </div> -->
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      studentProfile: [],
      id: [],
      studentCv: {},
      list: [],
      job: {
        type: Object,
        default: null,
      },
    };
  },

  methods: {
    deleteCV() {
      // call axios api delete cv
    },
  },

  mounted() {
    this.id = this.$route.query.id;
    if (localStorage.getItem("studentProfile")) {
      this.studentProfile = JSON.parse(localStorage.getItem("studentProfile"));
    }
    if (localStorage.getItem("token")) {
      this.token = localStorage.getItem("token");
    }
    axios.get("http://capstone2021-test.ap-southeast-1.elasticbeanstalk.com/student/cv/" + this.$route.query.id,
        {
          headers: {
            Authorization: `Bearer ${this.token}`,
          },
        }
    ).then(response => {
      this.studentCv = response.data.data;
    })
    axios
        .get("http://capstone2021-test.ap-southeast-1.elasticbeanstalk.com/job/applied-jobs", {
          headers: {
            Authorization: `Bearer ${this.token}`,
          },
        })
        .then((response) => {
          this.list = response.data.data;
        });
  },
};
</script>

<style>
</style>