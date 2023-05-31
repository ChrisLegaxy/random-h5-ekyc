<template>
  <div id="app">
    <div class="h-full flex justify-center leading-relaxed">
      <div
        class="w-full m-4 max-w-md flex flex-col items-center justify-evenly"
      >
        <div class="mt-6">
          <img class="aspect-square" :src="require('./assets/profile.svg')" />
        </div>
        <div class="text-center font-bold my-8">
          {{
            !language || language == "kh" ? content.kh.title : content.en.title
          }}
        </div>
        <div class="flex items-center mb-8">
          <div class="flex flex-col leading-loose gap-1">
            <div>
              <i class="fa-solid fa-circle-check text-green-500 mr-8" />
              <span
                >{{
                  !language || language == "kh"
                    ? content.kh.rule_1
                    : content.en.rule_1
                }}
              </span>
            </div>
            <div>
              <i class="fa-solid fa-circle-check text-green-500 mr-8" />
              <span>{{
                !language || language == "kh"
                  ? content.kh.rule_2
                  : content.en.rule_2
              }}</span>
            </div>
            <div>
              <i class="fa-solid fa-circle-check text-green-500 mr-8" /><span>{{
                !language || language == "kh"
                  ? content.kh.rule_3
                  : content.en.rule_3
              }}</span>
            </div>
            <div>
              <i class="fa-solid fa-circle-check text-green-500 mr-8" /><span>{{
                !language || language == "kh"
                  ? content.kh.rule_4
                  : content.en.rule_4
              }}</span>
            </div>
          </div>
        </div>
        <button
          class="bg-[#ff8300] mb-1 inline-block w-full p-2.5 mt-auto rounded-full font-bold text-white"
          @click="takeSelfie"
        >
          Scan Face
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import ZolozRealIdCore, { ekycLog } from "@/utils/zolozRealIdCore";
import queryString from "query-string";

const content = {
  kh: {
    title:
      "សូមធ្វើការស្កេនផ្ទៃមុខរបស់អ្នកដើម្បីបញ្ជាក់ថា អត្តសញ្ញាណ្ណដែលបានភ្ជាប់ និងម្ចាស់គណនី គឺជាបុគ្គលតែមួយ",
    rule_1: "សូមដោះវ៉ែនតា មួក និងម៉ាស់",
    rule_2: "សូមដាក់ផ្ទៃមុខអោយចំ និងនៅខាងក្នុងស៊ុមរូបភាព",
    rule_3: "សូមនៅកន្លែងដែលមានពន្លឺគ្រប់គ្រាន់",
    rule_4:
      "សូមធ្វើតាមការណែនាំ និងរង់ចាំសម្រាប់ការស្កេនផ្ទៀងផ្ទាត់ផ្សេងៗដោយស្វ័យប្រវត្តិ",
  },
  en: {
    title:
      "Please scan your face for us to ensure the submitted ID and the eWallet are of the same owner.",
    rule_1: "Remove glasses, hat and mask",
    rule_2: "Position your face within the frame",
    rule_3: "Make sure it's clear and bright enough",
    rule_4: "Follow instructions and wait for automatic scanning",
  },
};

export default {
  name: "app",
  data() {
    return {
      enableNext: true,
      zolozRealIdCore: new ZolozRealIdCore({}),
      language: undefined,
      content,
    };
  },
  mounted() {
    this.language = queryString.parse(location.search)["lang"];

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
  height: 100%;
  font-family: "Noto Sans", "Noto Sans Khmer", sans-serif;
}

body {
  margin: 0;
  padding: 0;
  height: 100%;
  /* font-size: 16px; */
}

#app {
  /* display: flex; */
  height: 100%;
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
