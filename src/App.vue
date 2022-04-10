<template>
  <div>
    <h1>Memory Game</h1>
    <section class="game-board">
      <AtomCard
        v-for="(card, index)in cardList"
        :key="`card-${index}`"
        :value="card.value"
        :visible="card.visible"
        :position="card.position"
        @select-card="flipCard"
      />
    </section>
  </div>
</template>

<script>
import { ref } from "vue";
import AtomCard from "./components/atom/AtomCard.vue";

export default {
  name: "App",
  components: {
    AtomCard,
  },
  setup() {
    const cardList = ref([]);

    for (let i = 0; i < 16; i++) {
      cardList.value.push({
        value: i,
        visible: false,
        position: i,
      });
    }

    const flipCard = (payload) => {
      cardList.value[payload.position].visible = true;
    };

    return {
      cardList,
      flipCard,
    };
  },
};
</script>

<style>
.game-board {
  display: grid;
  grid-template-columns: 100px 100px 100px 100px;
  grid-template-rows: 100px 100px 100px 100px;
  grid-column-gap: 30px;
  justify-content: center;
}
</style>
