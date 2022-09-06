<template>
  <div class="container">
    <input
      type="text"
      placeholder="Github User ID"
      :class="inputClasses"
      v-model.trim="userName"
      @keyup.enter="slideUpOut"
    />

    <div :class="userCardClasses">
      <template v-if="!this.infoError">
        <div class="user-card-baseinfo">
          <div class="user-card-profile">
            <a :href="this.userInfo.html_url" target="blank">
              <img :src="this.userInfo.avatar_url" alt="user profile" />
            </a>
          </div>
          <div class="user-card-userinfo">
            <div class="user-card-userinfo-username">
              <a :href="this.userInfo.html_url" target="blank">
                {{ this.userInfo.username }}
              </a>
            </div>
            <div class="user-card-userinfo-followers">
              followers: {{ this.userInfo.followers }}
            </div>
          </div>
        </div>

        <div class="user-card-repos">
          <div
            class="user-card-repo"
            v-for="(i, index) in this.userInfo.repos"
            :key="index"
          >
            <a :href="i.html_url" target="blank">
              {{ i.name }}
            </a>
          </div>
        </div>
      </template>

      <template v-else>
        <h1>Please refresh this page and retry</h1>
      </template>
    </div>
  </div>
</template>

<script>
import "animate.css";
import axios from "axios";

export default {
  name: "testName",

  data() {
    return {
      inputClasses: ["animate__animated"],
      userCardClasses: ["hide"],
      userName: "",
      userInfo: {},
      infoError: false,
    };
  },

  methods: {
    async slideUpOut() {
      if (this.userName.length !== 0) {
        await this.getUserInfo();
        this.inputClasses.push("animate__zoomOutUp");

        setTimeout(() => {
          this.userCardIn();
        }, 1000);
      }
    },

    async getUserInfo() {
      await axios
        .get(`https://api.github.com/users/${this.userName}`)
        .then((res) => {
          this.userInfo = {
            avatar_url: res.data.avatar_url,
            username: res.data.name,
            followers: res.data.followers,
            html_url: res.data.html_url,
            repos_url: res.data.repos_url,
            repos: [],
          };
        })
        .catch((err) => {
          console.error(err);
          this.infoError = true;
        });

      await axios
        .get(this.userInfo.repos_url)
        .then((res) => {
          for (let i = 0; i < 5; i++) {
            if (res.data[i].name.length <= 20) {
              const repo = res.data[i];
              this.userInfo.repos.push(repo);
            }
          }

          console.log(res.data[0]);
        })
        .catch((err) => {
          console.error(err);
          this.infoError = true;
        });
    },

    userCardIn() {
      this.userCardClasses = [
        "animate__animated",
        "animate__zoomInUp",
        "user-card",
      ];

      this.inputClasses.push("hide");
    },
  },
};
</script>

<style lang="less" scoped>
@import url("./base.css");
@import url("./input-style.css");
@import url("./user-card-style.css");
</style>
