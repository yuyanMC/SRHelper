<script setup lang="ts">
import {onMounted, ref, Ref} from "vue";
import {getLoginState, getUser, LoginState, User} from "./mihomo.ts";


const version = "2.0";

let user: Ref<User> = ref(getUser());

onMounted(() => {
  if (getLoginState() == LoginState.NoAccount) {
    alert("第一次使用请先登录！");
    loginPopup();
  }
});

function redirect(loc: string) {
  location.href = loc;
}

async function loginPopup() {
  let w = window.open(
      '/settings.html',
      'SRHelper',
      'height=600,width=800,top=300,left=200,toolbar=no,menubar=no, scrollbars=no,resizable=no,location=no, status=no'
  )
  await new Promise((resolve, reject) => {
    let _ = setInterval(() => {
      if (w.closed) {
        clearInterval(_);
        resolve();
      }
    }, 100);
  });
  location.reload();
}

</script>

<template>
  <div class="appBox">
    <div class="titleBox">
      <h1>SRHelper</h1>
    </div>
    <div class="box funcBox">
      <div class="cButton charBox" @click="redirect('/characters.html')">
        <span>角色练度</span>
      </div>
      <div class="cButton relicBox">
        <span>遗器评分</span>
      </div>
      <div class="cButton gachaBox">
        <span>抽卡分析</span>
      </div>
      <div class="cButton settingsBox" @click="loginPopup()">
        <span>设置</span>
      </div>
    </div>
    <div class="box infoBox">
      <img class="avatarImg" alt="头像" :src="'/srres/'+user.avatar"/>
      <p class="infoText">
        当前SRHelper适配游戏版本：{{ version }}
        <br/>
        请注意，SRHelper被设计用于1440x720的窗口大小，当前窗口大小为{{ $vssWidth }}x{{ $vssHeight }}
        <br/>
        名称：{{ user.name }}
        <br/>
        {{ user.description }}
      </p>
    </div>
    <div class="box supportBox">
      <p>
        帮帮我！金主先生！
        <br/>
        前往<a href="https://afdian.net/a/yuyanMC">爱发电</a>支持我的创作！
      </p>
    </div>
  </div>
</template>

<style scoped>
.appBox {
  background-color: #224;
  width: 100%;
  height: 100%;
  --margin: 25px;
}

.appBox {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-template-rows: 1fr 2fr 2fr;
  gap: 0 0;
  grid-auto-flow: row;
  grid-template-areas:
    "titleBox titleBox"
    "funcBox infoBox"
    "funcBox supportBox";
}

.titleBox {
  grid-area: titleBox;
}

.funcBox {
  grid-area: funcBox;
}

.infoBox {
  grid-area: infoBox;
}

.supportBox {
  grid-area: supportBox;
}

.titleBox > h1 {
  margin: 0;
  position: absolute;
  top: calc(10% - 0.5em);
  width: 100%;
  font-size: 40px;
}

.funcBox {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-template-rows: 1fr 1fr;
  gap: 20px 20px;
  grid-auto-flow: row;
  grid-template-areas:
    "charBox relicBox"
    "gachaBox settingsBox";
}

.charBox {
  grid-area: charBox;
}

.relicBox {
  grid-area: relicBox;
}

.settingsBox {
  grid-area: settingsBox;
}

.gachaBox {
  grid-area: gachaBox;
}

.infoBox {
  display: grid;
  grid-template-columns: 200px 1fr;
  grid-template-rows: 1fr;
  gap: 0 0;
  grid-auto-flow: row;
  grid-template-areas:
    "avatarImg infoText";
}

.avatarImg {
  margin: 10px;
  width: calc(100% - 20px);
  grid-area: avatarImg;
}

.infoText {
  grid-area: infoText;
  text-align: left;
}
</style>
