<template>
  <div id="app">
    <div class="mainContainer">
      <div class="mainText">
        Get ready to scan your face
      </div>
      <div class="imgContainer">
        <div class="zGifImg" :style="{'border-color':'#0f8ee9'}">
          <img class="zImg" :src="require('./assets/faceguide.gif')"/>
        </div>
      </div>
      <div class="mainText">
        Scan face tips
      </div>
      <div class="descText">
        • Position your face within the frame<br/>
        • Make sure it's clear and bright enough<br/>
        • Wait a moment for automatic scanning
      </div>
    </div>
    <div class='footer'>
      <div class="bottomButton" @click="takeSelfie" :style="{'background-color':'#0f8ee9', 'color':'#ffffff'}">
        Scan Face
      </div>
    </div>
  </div>
</template>

<script>
import ZolozRealIdCore, { ekycLog } from "@/utils/zolozRealIdCore";

export default {
  name: "app",
  data() {
    return {
      enableNext: true,
      zolozRealIdCore: new ZolozRealIdCore({}),
    };
  },
  mounted() {
    ekycLog("faceGuidePageAppear", {});
    document.addEventListener(
      "back",
      (e) => {
        e.preventDefault();
        ekycLog("clickButton", {
          onPage: "face_guide",
          clickItem: "back",
        });
        this.zolozRealIdCore.end("back");
      },
      false
    );
  },
  methods: {
    async takeSelfie() {
      // protect code
      if (this.enableNext) {
        ekycLog("faceGuidePageProceed");
        this.enableNext = false;
        await this.zolozRealIdCore.end("next");
        this.enableNext = true;
      }
    },
  },
};
</script>

<style>
html {
  font-size:100px;
}

body{
  margin: 0;
  padding: 0;
  color: #282828;
  font-size:16px;
}

#app {
  display: flex;
  min-height: 100vh;
  flex-direction: column;
}

.mainContainer {
  flex: 1;
  padding-top: 12px;
}

.footer {
  padding-top: 4px;
  padding-bottom: 16px;
  padding-left: 5%;
  padding-right: 5%;
}

.mainText {
  padding: 16px;
  font-size: 18px;
  font-weight: 700;
}

.descText {
  padding: 0 24px 8px;
  font-size: 14px;
  line-height: 1.8;
}

.imgContainer {
  width: 100%;
  text-align: center;
  margin-top: 16px;
}

.zGifImg {
  border-radius: 50%;
  border-style: solid;
  border-width: 5px;
  box-sizing: border-box;
  display: inline-block;
  overflow: hidden;
  width: 55%;
}

.zImg {
  border-radius: 50%;
  width: 100%;
  display: block;
}

.bottomButton {
  height: 0.4rem;
  line-height: 0.4rem;
  text-align: center;
  cursor: pointer;
}

</style>
