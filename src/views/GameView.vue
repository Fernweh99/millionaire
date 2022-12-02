<template>
  <div class="game">
    <div class="top">
      <router-link :to="{name: 'home'}"><i class="fa-solid fa-house home"></i></router-link>
      <div class="jackpot-container">
        <JackpotContainer :class="(index == currentJackpot ? 'active-prize' : '')" v-for="(prize, index) in jackpot" :key="prize" :prize="prize" v-show="(index == currentJackpot || index == currentJackpot -1 || index == currentJackpot +1)"/>
      </div>
      <img id="logo" src="../assets/image/logo.png" alt="" />
    </div>
    <div class="container">
      <Transition name="fade" mode="out-in">
        <ModalComponent v-if="hoverModal" :title="titleModal" :text="textModal" :state="stateModal" @gameWin="gameWin" @gameContinue="gameContinue" @newGame="newGame" @goHome="$router.push({ name: 'home' })"/>
      </Transition>
      <div class="content">
        <ContainerComponent v-if="collect.answer" :text="collect.answer" size="xl" />
        <div class="row">
          <div
            v-for="res, index in collect.responses"
            :key="res.label"
            class="col-12 col-md-6 d-flex justify-content-center"
          >
            <ContainerComponent class="res" :text="res.label" @click="result(res.state, index)"/>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import ContainerComponent from "@/components/ContainerComponent.vue";
import gamesDates from "@/assets/data/gamesDates";
import ModalComponent from '@/components/ModalComponent.vue';
import JackpotContainer from '@/components/jackpotContainer.vue';
export default {
  components: { ContainerComponent, ModalComponent, JackpotContainer },
  name: "GameView",
  data() {
    return {
      collect: "",
      querysFished: [],
      currentJackpot: -1,
      currentTrue: "",
      hoverModal: false,
      textModal: "",
      titleModal: "",
      stateModal: "",
      jackpot: [500, 1000, 2000, 5000, 10000, 50000, 100000, 200000, 500000, 1000000],
    };
  },
  methods: {
    result(state) {
      if (state){
        this.currentJackpot += 1;
        this.titleModal = "Risposta Esatta!";
        this.textModal = "Complimenti hai vinto "+ this.jackpot[this.currentJackpot] +"€ desideri continuare per provare a vincere "+ this.jackpot[this.currentJackpot + 1] + "€, o desideri fermarti?";
        this.stateModal = "continue"
      } else {
        const responses = this.collect.responses;
        console.log(responses);
        responses.forEach(item => {
          if(item.state) {
            this.currentTrue = item.label
          }
        });
        this.gameOver();
      } 
      if (this.querysFished.length == this.jackpot.length) {
        this.gameWin("1 milione di Euro!!");
      }
      else {
      this.openModal();
      }
    },
    gameWin(value = ""){
      this.titleModal = `Hai Vinto ${value}`;
      this.textModal = "Complimenti hai vinto "+ this.jackpot[this.currentJackpot] +"€ vuoi tornare alla home o iniziare una nuova partita?";
      this.stateModal = "gameover";
      this.openModal();
    },
    openModal () {
      this.hoverModal = true;
    },
    gameOver() {
      this.hoverModal = false;
      this.currentJackpot = -1;
      this.collect = "";
      this.querysFished = [];
      this.titleModal = `Hai Perso.. `;
      this.textModal = "Sfortunatamente hai sbagliato.. la risposta esatta era: '"+ this.currentTrue +"'  vuoi tornare alla home o iniziare una nuova partita?";
      this.stateModal = "gameover";
      this.openModal();
    },
    gameContinue(){
      this.hoverModal = false;
      this.getCollect();
    },
    newGame(){
      this.hoverModal = false;
      this.currentJackpot = -1;
      this.collect = "";
      this.querysFished = [];
      this.getCollect();
    },
    randomNumber() {
      let number = (Math.floor(Math.random() * (gamesDates.length - 0))) + 0;
      return number;
    },
    getCollect() {
      if (this.querysFished.length < this.jackpot.length) {
        
        let number = this.randomNumber();

        while (this.querysFished.includes(number)) {
          number = this.randomNumber();
        }
        this.querysFished.push(number);
        this.collect = gamesDates[number];
      } else {
        this.gameOver();
      }
    }
  },
  mounted() {
    this.getCollect()
  }
};
</script>

<style lang="scss" scoped>
  .content {
    background-color: #120939;
    display: flex;
    align-items: center;
    flex-direction: column;
  } .top {
    height: 350px;
    background-color: #0c0d89;
    padding: 30px 0;
    position: relative;
    .home {
      font-size: 55px;
      color: white;
      margin-left: 60px
    }
    #logo {
    height: 250px;
    position: absolute;
    top: 50%;
    left: 50%;
    width: 250px;
    transform: translate(-50%, -50%);
    }
    .jackpot-container {
      width: 120px;
      margin-left: 35px;
      margin-top: 40px;
      z-index: 10;
      display: flex;
      flex-direction: column;
      flex-flow: column-reverse;
      position: relative;
      &.jackpot-container > * {
        background-color: cornflowerblue;
      }
      .active-prize {
        background-color: black;
      }
    }
  }
  .res {
    cursor: pointer;
  }
</style>
