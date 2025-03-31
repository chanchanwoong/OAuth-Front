<template>
  <v-container>
    <v-row justify="center">
      <v-col md="4">
        <v-card>
          <v-card-title class="text-h5 text-center"> 로그인 </v-card-title>
          <v-card-text>
            <v-form>
              <v-text-field label="email" v-model="email"> </v-text-field>
              <v-text-field label="패스워드" v-model="password" type="password">
              </v-text-field>
              <v-btn type="button" color="primary" block @click="memberLogin()"
                >로그인</v-btn
              >
            </v-form>
            <br />
            <v-row>
              <v-col cols="6" class="d-flex justify-center">
                <img
                  src="@/assets/google_login.png"
                  style="max-height: 40px; width: auto"
                  @click="googleServerLogin()"
                />
              </v-col>
              <v-col cols="6" class="d-flex justify-center">
                <img
                  src="@/assets/kakao_login.png"
                  style="max-height: 40px; width: auto"
                  @click="kakaoLogin()"
                />
              </v-col>
              <v-col cols="6" class="d-flex justify-center">
                <img
                  src="@/assets/naver_login.png"
                  style="max-height: 40px; width: auto"
                  @click="naverLogin()"
                />
              </v-col>
            </v-row>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      email: "",
      password: "",
      googleUrl: "https://accounts.google.com/o/oauth2/auth",
      googleClientId:
        "21188799788-3okeo7f6e6molhkhc8aje0jmmmkiaam8.apps.googleusercontent.com",
      googleRedirectUrl: "http://localhost:3000/oauth/google/redirect",
      // openid는 요청하지 않아도 기본적으로 제공. email과 profile은 요청시 제공.
      googleScope: "openid email profile",
      kakaoUrl: "https://kauth.kakao.com/oauth/authorize",
      kakaoClientId: "65b59fbb09fe846b141256e50fcdb246",
      kakaoRedirectUrl: "http://localhost:3000/oauth/kakao/redirect",

      // naver 관련 정보
      naverUrl: "https://nid.naver.com/oauth2.0/authorize",
      naverClientId: "K1zYdLFiLlnhh2791oYu",
      naverRedirectUri: "http://localhost:3000/oauth/naver/redirect",
      naverState: "test",
    };
  },
  methods: {
    async memberLogin() {
      const loginData = {
        email: this.email,
        password: this.password,
      };
      const response = await axios.post(
        "http://localhost:8080/api/v1/member/login/google",
        loginData
      );
      const token = response.data.jwt;
      localStorage.setItem("token", token);
      window.location.href = "/";
    },
    googleServerLogin() {
      const auth_uri = `${this.googleUrl}?client_id=${this.googleClientId}&redirect_uri=${this.googleRedirectUrl}&response_type=code&scope=${this.googleScope}`;
      window.location.href = auth_uri;
    },
    kakaoLogin() {
      const auth_uri = `${this.kakaoUrl}?client_id=${this.kakaoClientId}&redirect_uri=${this.kakaoRedirectUrl}&response_type=code`;
      window.location.href = auth_uri;
    },
    naverLogin() {
      const auth_uri = `${this.naverUrl}?&client_id=${this.naverClientId}&redirect_uri=${this.naverRedirectUri}&state=${this.naverState}&response_type=code`;
      window.location.href = auth_uri;
    },
  },
};
</script>
