<template>
  <div class="card">
    <div v-if="visible" class="card-face is-front">
      {{ value }}
    </div>
    <div class="card-face is-back">Back</div>
  </div>
</template>

<script>
export default {
  props: {
    position: {
      type: Number,
      required: true,
    },
    value: {
      type: Number,
      required: true,
    },
    visible: {
      type: Boolean,
      default: false,
    },
  },
  setup(props, context) {
    const selectCard = () => {
      context.emit("select-card", {
        position: props.position,
      });
    };
    return {
      selectCard,
    };
  },

  visible: {
    type: Boolean,
    default: false,
  },
};
</script>

<style lang="scss" scoped>
.card {
  border: 5px solid #ccc;
  position: relative;
}

.card-face {
  width: 100%;
  height: 100%;
  position: absolute;
}

.card-face.is-front {
  background-color: red;
}
</style>
