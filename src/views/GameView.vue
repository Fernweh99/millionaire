<template>
  <div class="game">
    <div class="top">
      <img id="logo" src="../assets/image/logo.png" alt="" />
    </div>
    <div class="container">
      <ModalComponent v-if="hoverModal" :text="textModal" :state="stateModal" @gameOver="gameOver" @gameContinue="gameContinue" @goHome="$router.push({ name: 'home' })"/>
      <div class="content">
        <ContainerComponent v-if="collect.answer" :text="collect.answer" size="xl" />
        <div class="row">
          <div
            v-for="res in collect.responses"
            :key="res.label"
            class="col-12 col-md-6 d-flex justify-content-center"
          >
            <ContainerComponent class="res" :text="res.label" @click="result(res.state)"/>
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
export default {
  components: { ContainerComponent, ModalComponent },
  name: "GameView",
  data() {
    return {
      collect: "",
      querysFished: [],
      points: 0,
      hoverModal: false,
      textModal: "",
      stateModal: "",
    };
  },
  methods: {
    result(state) {
      if (state){
        this.points += 1;
        this.stateModal = "continue"
      } else {
        this.stateModal = "gameover"
      } 
      if (this.querysFished.length == gamesDates.length) {
        this.gameOver();
      }
      else {
        this.openModal();
      }
    },
    openModal () {
      this.hoverModal = true;
    },
    gameOver() {
      this.hoverModal = false;
      this.points = 0;
      this.collect = "";
      this.querysFished = [];
      this.stateModal = "gameover";
      this.openModal();
    },
    gameContinue(){
      this.hoverModal = false;
      this.getCollect();
    },
    randomNumber() {
      let number = (Math.floor(Math.random() * (gamesDates.length - 0))) + 0;
      return number;
    },
    getCollect() {
      if (this.querysFished.length < gamesDates.length) {
        
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
    background-color: #0c0d89;
    padding: 30px 0;
    display: flex;
    justify-content: center;
    #logo {
    height: 300px;
  }
  }
  .res {
    cursor: pointer;
  }
</style>
