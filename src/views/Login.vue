<template>
  <section id="login-section" class="container shadow-lg p-3 mb-5 rounded">
    <SysMsgSpace></SysMsgSpace>
    <form @submit.prevent="login()">
      <h1 style="text-align: center">CMS Admin Login</h1>
      <div class="form-group">
        <label for="email-login" class="label">
          <h4>Email address</h4>
        </label>
        <input
          type="email"
          class="form-control"
          id="email-login"
          v-model="user.email"
        />
      </div>
      <div class="form-group">
        <label for="pass-login">
          <h4>Password</h4>
        </label>
        <input
          type="password"
          class="form-control"
          id="pass-login"
          v-model="user.password"
        />
      </div>
      <div class="row justify-content-center mx-5">
        <button type="submit" class="btn btn-primary">Login</button>
      </div>
    </form>
  </section>
</template>

<script>
import cmsAPI from "../api/cms-api.js";
import SysMsgSpace from "../components/SysMsgSpace";
export default {
  name: "LoginPage",
  components: { SysMsgSpace },
  data() {
    return {
      user: {
        email: "",
        password: "",
      },
    };
  },
  methods: {
    toRegister() {
      this.$emit("changeDisplay", "register");
    },
    login() {
      cmsAPI({
        method: "POST",
        url: "/login",
        data: {
          email: this.user.email,
          password: this.user.password,
        },
      })
        .then(({ data }) => {
          localStorage.setItem("access_token", data.access_token);
          this.$store.commit("updateErrorMsg", null);
          this.$router.push({ name: "Dashboard" });
        })
        .catch(() => {
          this.$store.commit("updateErrorMsg", "Wrong ID/Password");
        })
        .finally(() => {
          this.user.email = "";
          this.user.password = "";
        });
    },
  },
  props: [],
  created() {
    if (localStorage.getItem("access_token")) {
      this.$router.push({ name: "Dashboard" });
    }
  },
};
</script>

<style>
</style>