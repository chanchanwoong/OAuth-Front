<template>
    <div>
        카카오 로그인 진행중...
    </div>
</template>

<script>
import axios from 'axios';

export default{
    created(){
        const code = new URL(window.location.href).searchParams.get("code");
        console.log(code);
        this.sendCodeToServer(code);
    },
    methods:{
        async sendCodeToServer(code){
            const response = await axios.post("http://localhost:8080/api/v1/member/login/kakao", {code});
            const token = response.data.jwt;
            localStorage.setItem("token", token);
            window.location.href = "/";
        }
    }
}
</script>