<template>
  <div class="game container justify-content-center align-items-center">
    <div class="game__media item align-self-stretch">
      <div class="game__media__img">
        <img :src="question.img" alt="Question Image">
      </div>
      <div class="game__media__audio">
        <audio :src="question.audio" controls></audio>
      </div>
    </div>
    <div class="game__question item align-self-stretch">
      <div class="game__question__text">
        {{ question.text }}
      </div>
      <div class="game__question__words">
        <span v-for="aswr in question.answers">{{ aswr.text }}</span>
      </div>
      <div class="game__question__options container align-items-center justify-content-between">
        <div class="game__question__options__item">
          Use the Keyboard above
        </div>
        <button class="game__question__options__item" :disabled="!currentWord.text" @click="clearWord()">
          Clear Word
        </button>
        <button class="game__question__options__item" :disabled="!currentWord.text" @click="setWord()">
          Next Word
        </button>
        <button class="game__question__options__item" :disabled="!ended || !currentWord.text" @click="answer(question)">
          Check
        </button>
      </div>
      <div class="game__question__keyboard container align-items-center justify-content-between">
        <!-- LIMIT 13 LETTERS -->
        <span v-for="letter in currentWord.letters" :class="{ disabled: letter.selected }" @click="chooseLetter(letter)">{{ letter.letter }}</span>
      </div>
      <div class="game__question__answer container column">
        <div class="game__question__answer__item container">
          <div class="game__question__answer__item__title container justify-content-center align-items-center">
            Your Word
          </div>
          <div class="game__question__answer__item__phrase item container justify-content-start align-items-center">
            <span v-for="aswr in question.answers" :class="{ wrong: (aswr.text !== aswr.correct) && question.answered }" >{{ aswr.text }}&nbsp</span>
          </div>
        </div>
        <div class="game__question__answer__item container">
          <div class="game__question__answer__item__title container justify-content-center align-items-center">
            Correct Word
          </div>
          <div class="game__question__answer__item__phrase item container justify-content-start align-items-center">
            {{ question.answered ? question.text : '' }}
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
      questions: [{
        img: 'https://www.worldatlas.com/r/w728-h425-c728x425/upload/4b/18/e8/tv-television-watching.jpg',
        audio: 'https://vignette.wikia.nocookie.net/leagueoflegends/images/3/3d/Taric.death01.ogg/revision/latest?cb=20160407223640',
        text: 'My light never dims',
        answered: false
      }],
      question: {},
      currentWord: {},
      ended: false
    }
  },
  computed: {
    // finished () {
    //   if (!this.question.answers) return false
    //   for (const a of this.question.answers) {
    //     console.log(a)
    //     if (!a.text) return false
    //     console.log(a.text)
    //   }
    //   return true
    // }
  },
  methods: {
    setCurrent () {
      for (const index in this.questions) {
        const qst = this.questions[index]
        if (!qst.answered || (index == (this.questions.length - 1))) {
          this.question = qst
          break
        }
      }
      if (!this.question.answers || !this.question.answers.length) {
        this.question.answers = this.question.text.replace(/[^a-zA-Z0-9\d\s:]/g, '').toLowerCase().split(' ').map(w => ({ correct: w, text: '' }))
      }
      this.setWord()
    },
    setWord () {
      if (!this.question.answers) this.currentWord = { correct: '', text:  '' }
      else {
        for (const index in this.question.answers) {
          const word = this.question.answers[index]
          if (!word.text) {
            this.currentWord = word
            this.setLetters()
            if (index == (this.question.answers.length - 1)) this.ended = true
            break
          }
        }
      }
    },
    setLetters () {
      if (!this.currentWord.letters) {
        const possibilities = 'abcdefghijklmnopqrstuvwxyz',
        currentWordLetters = this.currentWord.correct ? this.currentWord.correct.split('').map(l => ({ letter: l, selected: false })) : [],
        letters = [...currentWordLetters]

        while (letters.length < 13) {
          const randChar = possibilities.charAt(Math.floor(Math.random() * possibilities.length))
          if (!(letters.includes(randChar))) letters.push({ letter: randChar, selected: false })
        }
        this.$set(this.currentWord, 'letters', letters)
      }
    },
    chooseLetter (letter) {
      if (!letter.selected) {
        if (!this.currentWord.text) this.$set(this.currentWord, 'text', '')
        this.currentWord.text = `${this.currentWord.text}${letter.letter}`
        this.$set(letter, 'selected', true)
      }
    },
    clearWord () {
      this.$set(this.currentWord, 'text', '')
      for (let l of this.currentWord.letters) {
        l.selected = false
      }
    },
    answer (question) {
      this.$set(question, 'answered', true)
    }
  },
  mounted () {
    this.setCurrent()
  }
}
</script>

<style lang="scss" scoped>
.container {
  display: flex;
  &.column {
    flex-direction: column;
  }
  &.justify-content-center {
    justify-content: center;
  }
  &.justify-content-start {
    justify-content: flex-start;
  }
  &.justify-content-between {
    justify-content: space-between;
  }
  &.align-items-center {
    align-items: center;
  }
}

.item {
  flex-grow: 1;
  &.align-self-stretch {
    align-self: stretch;
  }
}

.game {
  padding: 20px;
  min-height: 400px;
  max-width: 1050px;
  &__media {
    flex-basis: 45%;
    &__img {
      img {
        max-width: 100%;
      }
    }
    &__audio {
      margin-top: 5px;
      audio {
        width: 100%;
      }
    }
  }
  &__question {
    flex-basis: calc(55% - 15px);
    margin-left: 15px;
    &__text {
      padding: 10px 20px;
      border: 2px solid #DDD;
      border-radius: 10px;
      margin-bottom: 10px;
    }
    &__words {
      padding: 10px 10px;
      border: 2px solid #DDD;
      border-radius: 10px;
      margin-bottom: 10px;
      display: flex;
      span {
        flex-grow: 1;
        flex-basis: 50px;
        padding: 5px 15px;
        margin: 5px;
        border-radius: 15px;
        background-color: #EEE;
        height: 18px;
        text-align: center;
        &:first-child {
          margin-left: 0px;
        }
        &:last-child {
          margin-right: 0;
        }
      }
    }
    &__options {
      margin-bottom: 10px;
      &__item {
        cursor: pointer;
        border: none;
        font-size: 16px;
        padding: 5px 15px;
        border-radius: 15px;
        background-color: #EEE;
        &.disabled, &[disabled] {
          cursor: not-allowed;
        }
      }
    }
    &__keyboard {
      padding: 10px;
      border: 2px solid #BBB;
      border-radius: 10px;
      background-color: #EEE;
      margin-bottom: 10px;
      span {
        cursor: pointer;
        padding: 5px 10px;
        border: 2px solid #777;
        border-radius: 5px;
        background-color: #BBB;
        color: #FFF;
        transition: all .2s;
        &.disabled {
          border-color: #DDD;
          background-color: #CCC;
          color: #777;
        }
      }
    }
    &__answer {
      &__item {
        &:first-child {
          margin-bottom: 5px;
        }
        &__title {
          padding: 10px 20px;
          border-radius: 5px;
          width: 100px;
          background-color: #DDD;
        }
        &__phrase {
          padding: 10px 20px;
          margin-left: 5px;
          border-radius: 5px;
          background-color: #DDD;
          span.wrong {
            color: red;
          }
        }
      }
    }
  }
}
</style>
