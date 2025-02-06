<template>
  <template v-if="dataUser">
    <p>{{dataUser}}</p>
  <button class="btn-confirm">CONFIRM</button>
  </template>
    <template v-else>
        <button class="btn-scan" @click="scan()">SCAN <img class="img-icon" src="../assets/Pictogrammers-Material-Qrcode-scan.512.png" alt=""></button>
  </template>
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


<style>
  .btn-scan{
    display: flex;
    flex-direction: column;
    gap:15px;
    align-items: center;
    margin: auto;
    cursor: pointer;
    font-size: 30px;
    padding:10px 30px;
    border:0;
    border-radius: 10px;
    color: #FFF;
    background-color: deepskyblue;
          transition-duration: 0.5s;


  }
    .btn-scan:hover{
      transition-duration: 0.5s;
    font-size: 35px;
  }

    .btn-confirm{
    display: flex;
    gap:15px;
    align-items: center;
    margin: auto;
    cursor: pointer;
    font-size: 30px;
    padding:10px 30px;
    border:0;
    border-radius: 10px;
    color: #FFF;
    background-color: green;
          transition-duration: 0.5s;


  }
    .btn-confirm:hover{
      transition-duration: 0.5s;
    font-size: 35px;
  }
    .img-icon{
      width: 50px;
    }
</style>