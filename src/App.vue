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
        {{ question.text }}
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
      }
    }
  }
}
</script>

<style lang="scss" scoped>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
