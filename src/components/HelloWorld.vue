<script setup lang="ts">
import {inject, ref} from "vue";


const phantom: any = inject("phantom");
const publicWalletAddress = ref("");
const abbreviatedAddress =  "Connected"

const connectPhantom = async () => {
  if (phantom) {
    const response = await phantom.connect();
    localStorage.setItem('isWalletConnected', true)
    publicWalletAddress.value = response.publicKey.toString();
    console.log('Connected with Public Key:', response)
  }
}
async function disconnect() {
  try {
    deactivate()
    localStorage.setItem('isWalletConnected', false)
  } catch (ex) {
    console.log(ex)
  }
}

</script>

<template>
  <div id="main-container">
    <template v-if="phantom && !publicWalletAddress">
      <button
          class="btn-mg"
          @click="connectPhantom"
      >
        CONNECT WALLET
      </button>
    </template>

    <template v-if="publicWalletAddress">
      <div>
        <p class="text-white-mg">
          Connected to the Solana network, <br/>
          <p>WalletID: {{ publicWalletAddress }}</p>
        </p>
       <button class="btn-mg">{{abbreviatedAddress}}</button>
        <div >
          <img src="static/images/welcome.gif" alt="welcome gif" />
        </div>
      </div>
    </template>

    <template v-if="phantom === null">
      <div class="no-phantom-wallet-container">
        <div >

          <div class="lds-dual-ring"></div>
          <p>Checking ...</p>
        </div>
        <a
            href="https://phantom.app/"
            target="_blank"
        >
          You don't have a Phantom wallet ! Get one there !
        </a>
      </div>
    </template>
  </div>




</template>

<style scoped>
#main-container {
  width: 100%;
  height: 90%;
  display: flex;
  justify-content: center;
  align-items: center;
}

#footer-container {
  width: 100%;
  height: 10%;
  display: flex;
  justify-content: center;
  align-items: center;
}

.btn-mg {
  border-radius: 5px;
  background-color: #4f46e5;
  color: white;
  font-weight: bolder;
  line-height: 1.5em;
  font-size: 20px;
  border: none;
  padding: 10px;
}

.github-link {
  color: white;
  text-decoration: none;
  font-weight: bolder;
}
.text-white-mg {
  color: white
}
.no-phantom-wallet-container {
  color: white;
  font-weight: bolder;
}
.no-phantom-wallet-container a {
  color: #4f46e5;
  font-weight: bolder;
  text-decoration: none;
}

.lds-dual-ring {
  display: inline-block;
  width: 80px;
  height: 80px;
}
.lds-dual-ring:after {
  content: " ";
  display: block;
  width: 64px;
  height: 64px;
  margin: 8px;
  border-radius: 50%;
  border: 6px solid #fff;
  border-color: #fff transparent #fff transparent;
  animation: lds-dual-ring 1.2s linear infinite;
}
@keyframes lds-dual-ring {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
</style>
