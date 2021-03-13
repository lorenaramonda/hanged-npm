<template>
  <div class="word2discover">
    <span v-for="(letter, i) in word" :key="i" class="letter">
      <span>{{ goodLetters.includes(letter) ? letter : '' }}</span>
    </span>
  </div>
</template>

<script>
export default {
  name: 'WordToDiscover',
  props: {
    word: {
      type: String,
      required: true
    },
    goodLetters: {
      type: Array,
      default: () => ([])
    }
  },
}
</script>

<style lang="scss" scoped>
.word2discover {
  margin: 10px auto;
  display: table;
  height: 4rem;
  display: flex;
  justify-content: center;
  margin-bottom: 4rem;

  .letter {
    border-bottom: solid 3px #979797;
    display: flex;
    justify-content: center;
  }

  .letter span {
    align-self: center;
  }
  .letter + .letter {
    margin-left: 7px;
  }
}
</style>