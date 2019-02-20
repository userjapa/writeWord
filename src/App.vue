<template>
  <div class="game">
    <div class="game__media">
      <div class="game__media__img">
        <img :src="question.img" alt="Question Image">
      </div>
      <div class="game__media__audio">
        <audio :src="question.audio" controls></audio>
      </div>
    </div>
    <div class="game__question">
      <div class="game__question__text">
        <div class="game__question__text__img">
          <img src="">
        </div>
        <div class="game__question__text__content">
          {{ question.text }}
        </div>
      </div>
      <div class="game__question__words">
        <span v-for="word in words">{{ word }}</span>
      </div>
      <div class="game__question__options">
        <div class="game__question__options__item">
          Use the Keyboard above
        </div>
        <div class="game__question__options__item">
          Clear Word
        </div>
        <div class="game__question__options__item">
          Next Word
        </div>
        <div class="game__question__options__item">
          Check
        </div>
      </div>
      <div class="game__question__keyboard">
        <!-- LIMIT 13 LETTERS -->
        <span v-for="letter in letters"></span>
      </div>
      <div class="game__question__answer">
        <div class="game__question__answer__item">
          <div class="game__question__answer__item__title">
            Your Word
          </div>
          <div class="game__question__answer__item__phrase">
            {{ question.answers }}
          </div>
        </div>
        <div class="game__question__answer__item">
          <div class="game__question__answer__item__title">
            Correct Word
          </div>
          <div class="game__question__answer__item__phrase">
            {{ question.text }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      question: {
        img: 'https://www.worldatlas.com/r/w728-h425-c728x425/upload/4b/18/e8/tv-television-watching.jpg',
        audio: 'https://vignette.wikia.nocookie.net/leagueoflegends/images/3/3d/Taric.death01.ogg/revision/latest?cb=20160407223640',
        text: 'My light never dims'
      },
      currentWord: 'my'
    }
  },
  computed: {
    words () {
      return this.question.text.replace(/[^a-zA-Z0-9\d\s:]/g, '').split(' ')
    },
    letters () {
      return []
    }
  }
}
</script>

<style lang="scss" scoped>
.game {
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
