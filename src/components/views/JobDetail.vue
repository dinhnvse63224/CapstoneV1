<template>
  <div id="job-detail">
    <!-- Page Header Start -->
    <div class="page-header">
      <div class="container">
        <div class="row">
          <div class="col-lg-12 col-md-6 col-xs-12">
            <div class="breadcrumb-wrapper">
              <div class="img-wrapper">
                <!-- <img v-bind:src="" /> -->
              </div>
              <div class="content">
                <p>{{ job.id }}</p>
                <h3 class="product-title">{{ job.name }}</h3>
                <p class="brand"></p>
                <div class="tags">
                  <span
                    ><i class="lni-map-marker"></i> {{ job.workingPlace }}</span
                  >
                  <span
                    ><i class="lni-calendar"></i> {{ job.createDate }}
                  </span>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- Page Header End -->

    <!-- Job Detail Section Start -->
    <section class="job-detail section">
      <div class="container">
        <div class="row justify-content-between">
          <div class="col-lg-8 col-md-12 col-xs-12">
            <div class="content-area">
              <h4>MÔ TẢ CÔNG VIỆC</h4>
              <li>{{ job.description }}</li>
              <h5>YÊU CẦU CÔNG VIỆC</h5>
              <li>{{ job.requirement }}</li>
              <h5>QUYỀN LỢI ĐƯỢC HƯỞNG</h5>
              <li>{{ job.offer }}</li>
              <br />
<!--              <router-link href="#" class="btn btn-common"-->
<!--                >Apply job</router-link-->
<!--              >-->
            </div>
          </div>
          <div class="col-lg-4 col-md-12 col-xs-12">
            <div class="sideber">
              <div class="widghet" v-if="!isShowInfo">
                <h3>THÔNG TIN TUYỂN DỤNG</h3>
                <button
                    @click.prevent="details"
                    class="btn btn-common log-btn"
                >
                  Chi tiết
                </button>
              </div>
              <div class="widghet" v-if="isShowInfo">
                <h3>THÔNG TIN TUYỂN DỤNG</h3>
                <li>
                  Mức lương: {{ job.salaryMin }} VNĐ - {{ job.salaryMax }} VNĐ
                </li>
                <br />
                <li v-if="job.workingForm == 1">
                  Hình thức làm việc: Full time
                </li>
                <li v-else>Hình thức làm việc: Part time</li>
                <br />
                <li>Số lượng cần tuyển: {{ job.quantity }}</li>
                <br />
                <li v-if="job.sex == 1">Yêu cầu giới tính: Nam</li>
                <li v-else>Yêu cầu giới tính: Nữ</li>
                <br />
              </div>
              <div class="widghet">
                <h3>CHIA SẺ VIỆC LÀM TỚI BẠN BÈ</h3>
                <div class="share-job">
                  <ul class="mt-4 footer-social">
                    <li>
                      <a class="facebook" href="#"
                        ><i class="lni-facebook-filled"></i
                      ></a>
                    </li>
                    <li>
                      <a class="twitter" href="#"
                        ><i class="lni-twitter-filled"></i
                      ></a>
                    </li>
                    <li>
                      <a class="linkedin" href="#"
                        ><i class="lni-linkedin-fill"></i
                      ></a>
                    </li>
                    <li>
                      <a class="google-plus" href="#"
                        ><i class="lni-google-plus"></i
                      ></a>
                    </li>
                  </ul>
                </div>
              </div>
            </div>
          </div>
        </div>
        <button
          type="button"
          class="btn btn-primary"
          @click.prevent="applyJob"
        >
          Nộp đơn
        </button>
        <button
            type="button"
            class="btn btn-primary"
            @click.prevent="saveJob"
        >
          Lưu việc
        </button>
        <div
          class="modal fade"
          id="exampleModal"
          tabindex="-1"
          aria-labelledby="exampleModalLabel"
          aria-hidden="true"
        >
          <div class="modal-dialog">
            <div class="modal-content">
              <div class="modal-header">
                <button
                  type="button"
                  class="btn-close"
                  data-bs-dismiss="modal"
                  aria-label="Close"
                ></button>
              </div>
              <div class="modal-body">
                <ul class="body-desc">
                  <li>Nộp đơn thành công. Vui lòng chờ nhà tuyển dụng.</li>
                </ul>
                <button
                  @click.prevent="accept"
                  class="btn btn-common log-btn"
                >
                  Xác nhận
                </button>
              </div>
            </div>
          </div>
        </div>
        <div
            class="modal fade"
            id="saveJobMessage"
            tabindex="-1"
            aria-labelledby="saveJobMessageLabel"
            aria-hidden="true"
        >
          <div class="modal-dialog">
            <div class="modal-content">
              <div class="modal-body">
                <ul class="body-desc">
                  <li>Lưu việc thành công. Vui lòng kiểm tra trong danh sách đã lưu.</li>
                </ul>
                <button
                    class="btn btn-common log-btn"
                    data-bs-dismiss="modal"
                >
                  Xác nhận
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
    <!-- Job Detail Section End -->
    <!-- Featured Section Start -->
    <section id="latest-jobs" class="section bg-gray">
      <div class="container">
        <div class="section-header">
          <h2 class="section-title">VIỆC LÀM TƯƠNG TỰ</h2>
        </div>
      </div>
    </section>
    <!-- Featured Section End -->
  </div>
</template>

<script>
import axios from "axios";
// import ListSuggestJob from "../Job/ListSuggestJob.vue";
export default {
  data() {
    return {
      job: {
        type: Object,
        default: null,
      },
      id: "",
      studentProfile: "",
      isShowInfo: false,
      token: "",
    };
  },
  components: {},
  mounted() {
    if (localStorage.getItem("studentProfile")) {
      this.studentProfile = JSON.parse(localStorage.getItem("studentProfile"));
    }
    this.token = localStorage.getItem("token")
    if (this.token) {
      this.isShowInfo = true;
    }
    axios
      .get("http://capstone2021-test.ap-southeast-1.elasticbeanstalk.com/job/" + this.$route.query.id)
      .then((response) => {
        this.job = response.data.data;
      });
  },
  methods: {
    // method ko nên dùng async
    applyJob() {
      if (this.token) {
        const header = {
          "Content-Type": "application/json",
          Authorization: `Bearer ${this.token}`,
        };
        const data = {
          id: Number(this.$route.query.id),
        };
        axios
          .post("http://capstone2021-test.ap-southeast-1.elasticbeanstalk.com/job/apply", data, {
            headers: header,
          })
          .then(() => {
            // eslint-disable-next-line no-undef
            $("#exampleModal").modal("show");
          })
          .catch((e) => {
            const { message } = e.response.data; 
              alert(message.toString());
          });
      } else {
        alert("Bạn cần đăng nhập để nộp đơn!");
      }
    },
    accept() {
      // this.$router.push('student-profile');
    },
    saveJob() {
      if (this.token) {
        const header = {
          "Content-Type": "application/json",
          Authorization: `Bearer ${this.token}`,
        };
        const data = {
          jobId: Number(this.$route.query.id),
        };
        console.log(data);
        axios
            .post("http://capstone2021-test.ap-southeast-1.elasticbeanstalk.com/job/save", data, {
              headers: header,
            })
            .then(() => {
              // eslint-disable-next-line no-undef
              $("#saveJobMessage").modal("show");
            })
            .catch((e) => {
              console.log(e.response);
              const { message } = e.response.data;
              alert(message.toString());
            });
      } else {
        alert("Bạn cần đăng nhập để nộp đơn!");
      }
    },
    details() {
      alert("Bạn cần đăng nhập để xem thông tin chi tiết");
    }
  },
};
</script>

<style>
</style>