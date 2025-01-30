<template>
  <div v-if="dataUser">
    <p>{{dataUser}}</p>
  <button>confirm</button>
  </div>
    <div v-else>
  <button @click="scan()">SCAN</button>
  </div>
</template>

<script setup>
import {ref,onMounted} from "vue";
import {liff} from "@line/liff";

const dataUser = ref()

 onMounted(async () => {
 console.log("mount")
    try {
      // * Config Line Liff
      await liff.init({ liffId: "2006835615-g520mXPq", withLoginOnExternalBrowser: true })
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
   liff
  .scanCodeV2()
  .then((result) => {
    console.log(result)
    dataUser.value = result
    // result = { value: "" }
  })
  .catch((error) => {
    alert(error)
    console.log("error", error);
  });
}
</script>
