<template>
  <div class="header">
    <!-- <h4>안녕 {{ $store.state.age[0] }}</h4> -->
    <!-- <button @click="$store.commit('숫자변경', 10)">변경</button> -->
    <!-- <p>{{ $store.state.more }}</p> -->
    <!-- <button @click="$store.dispatch('getData')">더보기</button> -->
    <ul class="header-button-left">
      <li @click="step = 0">Cancel</li>
    </ul>
    <ul class="header-button-right">
      <li v-if="step == 1" @click="step++">Next</li>
      <li v-if="step == 2" @click="publish">발행</li>
      <li v-if="step == 0" @click="step = 3">Followers</li>
    </ul>
    <img src="./assets/jinstagram.png" class="logo" />
  </div>

  <Container
    @write="작성한글 = $event"
    :이미지="이미지"
    :게시물="게시물"
    :step="step"
  />

  <button v-if="step == 0" class="more" @click="more">
    {{ 게시물더보기 }}
  </button>
  <div v-if="step == 0" class="footer">
    <ul class="footer-button-plus">
      <input
        @change="upload"
        multiple
        accept="image/*"
        type="file"
        id="file"
        class="inputfile"
      />
      <label for="file" class="input-plus">+</label>
    </ul>
  </div>
</template>

<script>
import Container from "./components/Container.vue";
import postdata from "./assets/postdata.js";
import axios from "axios";
import { mapMutations, mapState } from "vuex";
axios.post();

export default {
  name: "App",
  data() {
    return {
      게시물: postdata,
      더보기: 0,
      step: 0,
      이미지: "",
      작성한글: "",
      선택한필터: "",
      게시물더보기: "게시물 더보기",
    };
  },
  mounted() {
    this.emitter.on("박스클릭함", (a) => {
      this.선택한필터 = a;
    });
  },
  components: {
    Container: Container,
  },
  computed: {
    //계산결과저장용 함수들임
    name() {
      return this.$store.state.name;
    },
    age() {
      return this.$store.state.age;
    },
    // ...mapState(["name", "age", "likes"]),
    ...mapState({ 내이름: "name" }),
  },
  methods: {
    ...mapMutations(["setMore", "좋아요"]),
    now() {
      return new Date();
    },
    publish() {
      var 내게시물 = {
        name: "xx_jwoo",
        userImage: "https://placeimg.com/100/100/arch",
        postImage: this.이미지,
        likes: 36,
        date: "May 15",
        liked: false,
        content: this.작성한글,
        filter: this.선택한필터,
      };
      this.게시물.unshift(내게시물);
      this.step = 0;
    },
    more() {
      axios
        .post("URL", { name: "kim" })
        .then()
        .catch((err) => {
          err;
        });

      axios
        .get(`https://codingapple1.github.io/vue/more${this.더보기}.json`)
        .then((결과) => {
          this.게시물.push(결과.data);
          this.더보기++;
        });
      if (this.더보기 == 2) {
        this.게시물더보기 = "끝";
      }
    },
    upload(e) {
      let 파일 = e.target.files;
      console.log(파일[0].type);
      let url = URL.createObjectURL(파일[0]);
      console.log(url);
      this.이미지 = url;
      this.step++;
    },
  },
};
</script>

<style>
@import "style.css";

body {
  margin: 0;
}
ul {
  padding: 5px;
  list-style-type: none;
}
.logo {
  height: 30px;
  margin: auto;
  display: block;
  position: absolute;
  left: 0;
  right: 0;
  top: 13px;
}
.header {
  width: 100%;
  height: 40px;
  background-color: white;
  padding-bottom: 8px;
  position: sticky;
  top: 0;
  z-index: 100;
}
.header-button-left {
  color: skyblue;
  float: left;
  width: 50px;
  padding-left: 20px;
  cursor: pointer;
  margin-top: 10px;
}
.header-button-right {
  color: skyblue;
  float: right;
  width: 74px;
  cursor: pointer;
  margin-top: 10px;
}
.footer {
  width: 100%;
  position: sticky;
  bottom: 0;
  padding-bottom: 10px;
  background-color: rgba(255, 255, 255, 0.5);
}
.more {
  position: absolute;
  left: 50%;
  transform: translate(-50%, -50%);
  padding: 5px 10px;
  text-align: center;
  border-radius: 15px;
  border: none;
  background: #efefef;
  font-weight: 700;
  cursor: pointer;
  box-shadow: 0px 0px 4px #aaa;
  z-index: 100;
}
.footer-button-plus {
  width: 80px;
  margin: auto;
  text-align: center;
  cursor: pointer;
  font-size: 24px;
  padding-top: 12px;
}
.sample-box {
  width: 100%;
  height: 600px;
  background-color: bisque;
}

.inputfile {
  display: none;
}
.input-plus {
  cursor: pointer;
}
#app {
  box-sizing: border-box;
  font-family: sans-serif;
  margin-top: 60px;
  width: 100%;
  max-width: 460px;
  margin: auto;
  position: relative;
  border-right: 1px solid #eee;
  border-left: 1px solid #eee;
}
</style>
