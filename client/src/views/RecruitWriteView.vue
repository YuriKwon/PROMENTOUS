<template>
  <div>
    <div class="container mt-5">
      <form class="row g-3">
        <div class="row mb-5">
          <div class="title col-sm-10">프로젝트 생성</div>
        </div>
        <div class="row mb-5">
          <label for="projectName" class="col-sm-2 col-form-label"
            >프로젝트명</label
          >
          <div class="col-sm-3">
            <input
              type="text"
              class="form-control"
              id="projectName"
              v-model="PROJECT_TITLE" />
          </div>
        </div>
        <div class="row mb-5">
          <label for="inputStartDate" class="col-sm-2 col-form-label"
            >시작 예정일</label
          >
          <div class="col-sm-3">
            <input
              type="datetime-local"
              name=""
              id=""
              v-model="EXP_START_DATE" />
          </div>

          <label for="onoff" class="col-sm-2 col-form-label">진행기간</label>
          <div class="col-sm-3">
            <select id="onoff" class="form-select" v-model="EXP_PERIOD">
              <option disabled>예상기간을 선택해주세요!</option>
              <option value="1">1개월</option>
              <option value="2">2개월</option>
              <option value="3">3개월</option>
              <option value="4">4개월</option>
              <option value="5">5개월</option>
              <option value="6">6개월 이상</option>
            </select>
          </div>
        </div>
        <div class="row mb-5">
          <label class="col-2">팀장 분야<br /> </label>
          <div class="col partTo" v-show="this.PartAdd == 'x'">
            <input
              type="text"
              class="form-control"
              style="width: 200px"
              placeholder=""
              v-model="LEADER_DEPT_NAME" />

            <button type="button" class="btn btn-secondary" @click="addPart2()">
              추가
            </button>
          </div>
          <div class="col" v-show="this.PartAdd == 'o'">
            <div class="col partTo ms-1">
              <p class="form-control mb-1">
                {{ LEADER_DEPT_NAME }}
              </p>

              <button
                type="button"
                class="btn btn-secondary"
                @click="delPart2()">
                X
              </button>
            </div>
          </div>
        </div>
        <div class="row mb-1">
          <label for="reqNum" class="col-sm-2">모집인원</label>

          <div class="col partTo">
            <input
              type="text"
              class="form-control"
              style="width: 200px"
              placeholder="분야"
              v-model="DEPT.apply_dept_code" />

            <input
              type="number"
              class="form-control"
              style="width: 200px"
              placeholder="인원"
              v-model="DEPT.to"
              oninput="this.value=this.value.replace(/[^0-9]/g,'');" />

            <button type="button" class="btn btn-secondary" @click="addPart()">
              추가
            </button>
          </div>
          <div class="row" v-for="(DEPT, index) in DEPT_LIST" :key="index">
            <label for="reqNum" class="col-sm-2"> </label>

            <div class="col partTo ms-1">
              <p class="form-control mb-1">
                {{ DEPT_LIST[index].apply_dept_code }}
              </p>
              <p class="form-control mb-1">
                {{ DEPT_LIST[index].to }}
              </p>
              <button
                type="button"
                class="btn btn-secondary"
                @click="delPart(index)">
                X
              </button>
            </div>
          </div>
        </div>

        <div class="row mt-5 mb-5">
          <label for="onoff" class="col-sm-2 col-form-label">진행방식</label>
          <div class="col-sm-3">
            <select id="onoff" class="form-select" v-model="PROGRESS_METHOD">
              <option disabled>진행방식을 선택해주세요!</option>
              <option value="ON">온라인</option>
              <option value="OFF">오프라인</option>
            </select>
          </div>
          <div class="col" v-show="PROGRESS_METHOD == 'OFF'">
            <label for="region" class="col-sm-2 col-form-label me-5"
              >진행 지역</label
            >
            <RegionSortLayout
              class="col ms-5"
              style="display: inline"
              @send-LargeCity="SendLargeCity"
              @send-RestCity="SendRestCity" />
          </div>
        </div>
        <div class="row mb-5">
          <label class="col-sm-2 col-form-label">보증금 여부</label>
          <div class="col-sm-7 form-check">
            <div>
              <input
                type="radio"
                id="O"
                name="contact"
                value="O"
                v-model="WARRANTY"
                class="me-2" />
              <label for="O" class="me-3">O</label>
              <input
                class="me-3 form-select warranty"
                type="number"
                v-model="WARRANTY"
                v-show="WARRANTY !== '-1' && WARRANTY !== ''" />
              <input
                type="radio"
                id="X"
                value="-1"
                name="contact"
                v-model="WARRANTY"
                class="me-2" />
              <label for="X" class="me-3">X</label>
            </div>
          </div>
        </div>
        <div class="row mb-5">
          <label for="stack" class="col-sm-2 col-form-label"
            >사용 기술/언어 <br />
          </label>
          <StackSearchLayout
            class="col-sm-3"
            id="stack"
            @send-value="sendValue" />
        </div>

        <div class="row mb-5">
          <label class="col-sm-10 col-form-label"
            >연락 방법<br />
            <p>
              * 연락을 위한 URL(카카오톡 오픈 톡방 링크 등)또는 이메일을
              입력해주세요.
            </p></label
          >
          <input
            type="text"
            class="form-control"
            v-model="MEETING_URL"
            placeholder="ex)www.naver.com" />
        </div>
        <div class="row mb-5">
          <label class="col-sm-10 col-form-label"
            >참고 링크<br />
            <p>
              * 프로젝트와 관련해 참고할 수 있는 사이트를 입력해주세요.
            </p></label
          >
          <div class="col partTo">
            <input
              type="text"
              class="form-control"
              style="width: 200px"
              placeholder="링크 이름"
              v-model="URL.url_title" />

            <input
              type="text"
              class="form-control"
              style="width: 500px"
              placeholder="링크 주소 ex) www.naver.com"
              v-model="URL.url_address" />
            <button type="button" class="btn btn-secondary" @click="addUrl()">
              추가
            </button>
          </div>
          <div class="row" v-for="(URL, index) in URL_LIST" :key="index">
            <div class="col partTo ms-1">
              <p class="form-control mb-1">
                {{ URL_LIST[index].url_title }}
              </p>
              <p class="form-control mb-1">
                {{ URL_LIST[index].url_address }}
              </p>
              <button
                type="button"
                class="btn btn-secondary"
                @click="delURL(index)">
                X
              </button>
            </div>
          </div>
        </div>
        <div class="row mb-5">
          <text-editor :contents="CONTENTS" @send-EditorData="sendEditorData" />
        </div>
      </form>
      <registerbtn-layout
        class="regBtn"
        :btnText="btnText"
        @click="insertProject" />
    </div>
  </div>
</template>
<script>
import "@vuepic/vue-datepicker/dist/main.css";
import RegionSortLayout from "@/components/layouts/RegionSortLayout.vue";
import StackSearchLayout from "@/components/layouts/StackSearchLayout.vue";
import TextEditor from "@/components/TextEditor.vue";
import RegisterbtnLayout from "../components/layouts/RegisterbtnLayout.vue";
export default {
  name: "app",
  components: {
    RegionSortLayout,
    StackSearchLayout,
    TextEditor,
    RegisterbtnLayout
  },
  data() {
    return {
      LEADER_DEPT_NAME: "",
      DEPT_LIST: [],
      PartAdd: "x",
      DEPT: { apply_dept_code: "", to: 0 },
      URL: { url_title: "", url_address: "" },
      URL_LIST: [],
      TOTAL_TO: 0,
      PROJECT_ID: "",
      PROJECT_TITLE: "",
      EXP_START_DATE: "",
      EXP_PERIOD: 0,
      PROGRESS_METHOD: "",
      WARRANTY: "",
      MEETING_URL: "",
      SELECTED_PART: "",
      SELECTED_TO: "",
      DATE: "",
      STACKS: "",
      btnText: "작성 완료",
      CONTENTS:
        "<h1>1. 프로젝트 주제</h1> <h3>    -프로젝트 내용, 시작 동기 등에 관해 적어주세요!</h3><br><br><h1>2. 모임 방식/ 진행 방법</h1> <h3>    -모임을 1주일에 몇 번 정도 진행할 지 적어주세요!</h3><br><h3>    -모임 진행을 희망하는 요일을 적어주세요!</h3><br><h3>    -모임 진행 방식에 대해 상세히 적어주세요!</h3><br><br><h1>3. 그 외 자유 작성 사항</h1>",
      //  "1. 프로젝트 주제  2. 모임 방식/ 진행 방법 3.그 외 자유 작성 사항"
      PROJECT_DESC: "",
      MAIN_AREA_CODE: "",
      SUB_AREA_CODE: ""
    };
  },
  setup() {},
  created() {},
  mounted() {},
  unmounted() {},
  methods: {
    sendValue(data) {
      this.STACKS = data;
    },
    sendEditorData(data) {
      this.PROJECT_DESC = data;
    },
    addPart() {
      if (
        this.DEPT.apply_dept_code !== "" &&
        this.DEPT.to !== 0 &&
        this.DEPT.to < 10 &&
        this.TOTAL_TO < 10 &&
        this.TOTAL_TO + this.DEPT.to < 10
      ) {
        this.TOTAL_TO += this.DEPT.to;
        let obj = {
          ["apply_dept_code"]: this.DEPT.apply_dept_code,
          ["to"]: this.DEPT.to
        };
        this.DEPT_LIST.push(obj);
        this.DEPT.apply_dept_code = "";
        this.DEPT.to = 0;
      } else if (this.DEPT.apply_dept_code === "" || this.DEPT.to === 0) {
        alert("분야, 인원을 정확히 입력해주세요");
      } else if (
        this.TOTAL_TO > 9 ||
        this.DEPT.to > 9 ||
        this.TOTAL_TO + this.DEPT.to >= 10
      ) {
        alert("인원은 최대 9명 모집 가능합니다.");
      }
    },
    addUrl() {
      if (this.URL.url_title !== "" && this.URL.url_address !== "") {
        let obj0 = {
          ["url_title"]: this.URL.url_title,
          ["url_address"]: this.URL.url_address
        };
        this.URL_LIST.push(obj0);
        this.URL.url_title = "";
        this.URL.url_address = "";
      } else if (this.URL.url_title === "" || this.URL.url_address === 0) {
        alert("링크를 정확히 입력해주세요");
      }
    },

    delPart(index) {
      this.TOTAL_TO -= this.DEPT_LIST[index].to;
      this.DEPT_LIST.splice(index, 1);
    },
    delURL(index) {
      this.URL_LIST.splice(index, 1);
    },
    addPart2() {
      this.PartAdd = "o";
    },
    delPart2() {
      this.LEADER_DEPT_NAME = "";
      this.PartAdd = "x";
    },
    SendLargeCity(data) {
      this.MAIN_AREA_CODE = data;
    },
    SendRestCity(data) {
      this.SUB_AREA_CODE = data;
    },
    // 온라인 요청과 오프라인 요청을 나누어서 보내준다. 우선 아래는 온라인
    async insertProject() {
      let response = [];
      if (
        this.LEADER_DEPT_NAME === "" ||
        this.PROJECT_TITLE === "" ||
        this.PROJECT_DESC === "" ||
        this.EXP_PERIOD === 0 ||
        this.DEPT_LIST === [] ||
        this.PROGRESS_METHOD === "" ||
        this.WARRANTY === "" ||
        this.STACKS === "" ||
        this.MEETING_URL === ""
      ) {
        this.$swal("작성하지 않은 부분이 있어요!");
      } else if (this.PROGRESS_METHOD === "ON") {
        response = await this.$post(
          `http://localhost:3000/project/recruit/insertPost`,
          {
            param: {
              leader_user: this.$store.state.user.user_id,
              title: this.PROJECT_TITLE,
              exp_start_date: this.EXP_START_DATE,
              exp_period: parseInt(this.EXP_PERIOD),
              progress_method: this.PROGRESS_METHOD,
              stack_code: this.STACKS.join(),
              project_contact: this.MEETING_URL,
              project_desc: this.PROJECT_DESC,
              warranty: parseInt(this.WARRANTY),
              leader_dept: this.LEADER_DEPT_NAME
            }
          }
        );
        console.log(response);
      } else {
        response = await this.$post(
          `http://localhost:3000/project/recruit/insertPost`,
          {
            param: {
              leader_user: this.$store.state.user.user_id,
              title: this.PROJECT_TITLE,
              exp_start_date: this.EXP_START_DATE,
              exp_period: parseInt(this.EXP_PERIOD),
              progress_method: this.PROGRESS_METHOD,
              main_area_code: this.MAIN_AREA_CODE,
              sub_area_code: this.SUB_AREA_CODE,
              stack_code: this.STACKS.join(),
              project_contact: this.MEETING_URL,
              project_desc: this.PROJECT_DESC,
              warranty: parseInt(this.WARRANTY),
              leader_dept: this.LEADER_DEPT_NAME
            }
          }
        );
      }
      const postId = response.data.insertId;
      for (let i = 0; i < this.URL_LIST.length; i++) {
        this.URL_LIST[i].post_category = "RCB";
        this.URL_LIST[i].post_id = postId;
      }
      for (let i = 0; i < this.DEPT_LIST.length; i++) {
        this.DEPT_LIST[i].project_id = postId;
      }
      await this.insertAdditional();
      this.$router.push("/project/recruit");
    },
    async insertAdditional() {
      const response = await this.$post(
        `http://localhost:3000/project/recruit/insertAdditional`,
        {
          param: [this.URL_LIST, this.DEPT_LIST]
        }
      );
      console.log(response);
    }
  }
};
</script>
<style scoped>
.regBtn {
  float: right;
}
.title {
  font-size: 40px;
  font-weight: bold;
}
.warranty {
  width: 150px;
  display: inline;
}
.partTo > input {
  display: inline;
  margin-right: 5px;
}
.col.partTo {
  position: relative;
  bottom: 5px;
}
p.form-control {
  width: 200px;
  display: inline-block;
  margin-right: 5px;
}
</style>
