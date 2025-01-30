<template>
  SCAN
  <button @click="scan()">SCAN</button>
</template>

<script setup>
import {onMounted} from "vue";
import {liff} from "@line/liff";

 onMounted(async () => {
 console.log("mount")
    try {
      // * Config Line Liff
      await liff.init({ liffId: "2006835240-LbMy0qgq", withLoginOnExternalBrowser: true })
      // ^ Check Line Liff Login ?
      if (liff.isLoggedIn()) {
        const profile = await liff.getProfile()
        console.log("profile => ", profile)
      } else {
        liff.login()
      }
    } catch (e) {
      console.log("error login => ", e)
    }
})

function scan(){
   if (liff.scanCode) {
  liff.scanCode().then((result) => {
    console.log(result)
    // result = { value: "" }
  });
}
}
</script>
