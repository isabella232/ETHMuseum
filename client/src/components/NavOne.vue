<template>
  <div id="nav-container">
    <div class="logo-block">
      <img
        class="block-logo"
        src="../assets/images/icon-dapp-eth-blocks.svg"
        alt=""
      />
      <h1>ETH Museum</h1>
      <p class="slogan">Own a piece of Ethereum History</p>
    </div>

    <div>
      <button
      v-if="hasNotConnected"
      class="connect-btn"  
      @click="connectWallet">
        Connect Wallet
      </button>

      <button 
      v-else
      class="disconnect-btn"
      @click="disConnectWeb3">
        Disconnect Wallet
      </button>
    </div>
  </div>
</template>

<script>
/* eslint-disable */
export default {
  name: "NavOne",
  props: {
    walletId: String,
  },
  data() {
    return {
      hasNotConnected: true,
    };
  },
  methods: {
    connectWallet(e) {
      e.preventDefault();
      console.log('attempting to connect from NavOne')
      this.$emit("connectWallet", true);
    },
    disConnectWeb3(e) {
      e.preventDefault();
      this.$emit("disconnectWeb3")
    }
  },
  watch: {
    walletId(newVal) {
      if (newVal !== '') {
        this.hasNotConnected = false;
      } else {
        this.hasNotConnected = true;
      }
    },
  }
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Roboto:wght@400;500&display=swap");

html,
body,
#nav-container {
  font-family: "Roboto";
}

#nav-container {
  width: 100%;
  height: 50%;
  background-color: #ffffff;
  border-bottom: 1px solid #f8f9fb;
  /* box-shadow: 5px 5px 10px rgba(100, 100, 100, 0.062);; */
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
}

.logo-block {
  width: 490px;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  padding-left: 7.7%;
  /* border: 1px solid black; */
}

h1 {
  width: 210px;
  color: #05c0a5;
}

p {
  width: 220px;
  color: #5a678a;
  font-size: 15px;
  padding-top: 10px;
}

img {
  width: 30px;
  height: 30px;
}

input {
  width: 280px;
  height: 31px;
  border-radius: 10px;
  border: 1px solid #d7dae3;
  padding-left: 8px;
  border-top-left-radius: 8px;
  border-bottom-left-radius: 8px;
  border-top-right-radius: 0px;
  border-bottom-right-radius: 0px;
  color: #192133;
}

input::placeholder {
  color: #939fb9;
  opacity: 0.7;
}

button {
  width: 80px;
  height: 35px;
  border-top-left-radius: 0px;
  border-bottom-left-radius: 0px;
  border-top-right-radius: 8px;
  border-bottom-right-radius: 8px;
  border: none;
  background-color: #05c0a5;
  color: white;
}

button:hover {
  transition: ease-in-out 200ms;
  cursor: pointer;
  background-color: #0bd9ba;
}

.connect-btn {
  width: 180px;
  height: 35px;
  border-radius: 30px;
  border: none;
  margin-right: 35px;
}

.disconnect-btn {
  width: 180px;
  height: 35px;
  border-radius: 30px;
  border: none;
  margin-right: 35px;
  /* display: none; */
}
</style>
