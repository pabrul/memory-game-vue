<template>
  <div>
    <h1>Memory Game</h1>
    <section class="game-board">
      <AtomCard
        v-for="(card, index)in cardList"
        :key="`card-${index}`"
        :matched="card.matched"
        :value="card.value"
        :visible="card.visible"
        :position="card.position"
        @select-card="flipCard"
      />
    </section>
    <h2>{{ userSelection }}</h2>
    <p>remaining Pairs: {{ remainingPairs }}</p>
  </div>
</template>

<script>
import { ref, watch, computed } from "vue";
import AtomCard from "./components/atom/AtomCard.vue";

export default {
  name: "App",
  components: {
    AtomCard,
  },
  setup() {
    const cardList = ref([]);
    const userSelection = ref([]);
    const status = computed(() => {
      if (remainingPairs.value === 0) {
        return "Player Wins!";
      } else {
        return `Remaining Pairs: ${remainingPairs.value}`;
      }
    });

    const remainingPairs = computed(() => {
      const remainingCards = cardList.value.filter(
        (card) => card.matched === false
      ).length;

      return remainingCards / 2;
    });

    for (let i = 0; i < 16; i++) {
      cardList.value.push({
        value: i,
        visible: false,
        position: i,
        matched: false,
      });
    }

    const flipCard = (payload) => {
      cardList.value[payload.position].visible = true;

      if (userSelection.value[0]) {
        userSelection.value[1] = payload;
      } else {
        userSelection.value[0] = payload;
      }
    };

    watch(
      userSelection,
      (currentValue) => {
        if (currentValue.length === 2) {
          const cardOne = currentValue[0];
          const cardTwo = currentValue[1];

          if (cardOne.faceValue === cardTwo.faceValue) {
            status.value = "Matched!";

            cardList.value[cardOne.position].matched = true;
            cardList.value[cardTwo.position].matched = true;
          } else {
            status.value = "Mismatch";

            cardList.value[cardOne.position].visible = false;
            cardList.value[cardTwo.position].visible = false;
          }
          userSelection.value.length = 0;
        }
      },
      { deep: true }
    );

    return {
      cardList,
      flipCard,
      userSelection,
      status,
    };
  },
};
</script>

<style>
.game-board {
  display: grid;
  grid-column-gap: 30px;
  grid-template-columns: 100px 100px 100px 100px;
  grid-template-rows: 100px 100px 100px 100px;
  justify-content: center;
}
</style>
