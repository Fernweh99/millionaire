<template>
  <div class="modal d-block" tabindex="-1">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title">Modal title</h5>
      </div>
      <div class="modal-body">
        <p>{{text}}</p>
      </div>
      <div v-if="state=='continue'" class="modal-footer justify-content-center">
        <button type="button" @click="$emit('gameOver')" class="btn btn-secondary">{{textBtn1}}</button>
        <button type="button" @click="$emit('gameContinue')" class="btn btn-primary">{{textBtn2}}</button>
      </div>
      <div v-if="state=='gameover'" class="modal-footer justify-content-center">
        <button type="button" @click="$emit('goHome')" class="btn btn-secondary">{{textGameover1}}</button>
        <button type="button" @click="$emit('gameContinue')" class="btn btn-primary">{{textGameover2}}</button>
      </div>
    </div>
  </div>
</div>
</template>

<script>
export default {
  name: "ModalComponent",
  props: {
    text: String,
    state: String,
  },
  data(){
    return {
      textBtn1: "Incassa",
      textBtn2: "Continua",
      textGameover1: "Torna alla Home",
      textGameover2: "Nuova Partita",
    }
  },
  mounted(){
    if (this.state == "continue") {
      this.textBtn1 = "Incassa";
      this.textBtn2 = "Continua";
    } else if (this.state == "gameover") {
      this.textBtn1 = "Torna alla Home";
      this.textBtn2 = "Nuova Partita";
    }
  }
}
</script>

<style>

</style>