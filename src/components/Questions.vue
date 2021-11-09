<template>
  <div class="container">
    <div 
    class="card"
    v-bind:class = "{disabled}"
    >
      <h1>{{title}} </h1>
      <form onsubmit = "alert('Ответ отправлен');return false"
      method="POST"
      v-on:click.stop
      v-show="index === questionIndex"
      v-for="(question,index) in data"
      :key="question.id"
      >
      <h2>Категория:{{question.category}}</h2>
      <h2>Сложность:{{question.difficulty}}</h2>
      <h2>Вопрос:{{question.question}}</h2>
        <h2>Варианты ответов: 
          <br>
          <label class="answer">
            <input 
            v-on:click.once="userResponse++"
            v-on:click="next"
            name="variants"
            type="submit" 
            v-show="false"
            > - {{question.correct_answer}}
            </label>
            <li v-for="answer in question.incorrect_answers" :key="answer.id"> 
              <label class="answer" >
              <input 
              name="variants" 
              type="submit" 
              id="answer.id"
              v-show="false"
              v-on:click="next"
              > - {{answer}}
              </label>
              </li>
        </h2>
        <br>
      </form>
      <button 
        @click = "visible = !visible"
        @click.stop = "disabled = !disabled, rebootInvis = !rebootInvis "
      >Завершить</button>
    </div>

    <h1 v-if="visible">Правильных ответов: {{userResponse}}</h1>
    <button 
    v-bind:class = "{rebootInvis}"
    onClick="history.go(0)"
    class="reboot">Начать заново?</button>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'Questions',
  data() {
    return {
      title: "Список вопросов",
      data: null,
      questionIndex: 0,
      userResponse: 0,
      visible: false,
      disabled: false,
      rebootInvis: true,
    }
  },
  methods: {
    next: function() {
      this.questionIndex++;
    },
  },
  mounted() {
    axios
      .get('https://opentdb.com/api.php?amount=50&type=multiple')
      .then(response => (this.data = response.data.results))
  }
}

</script>

<style  lang="scss" scoped>

  .disabled {
    pointer-events: none;
    opacity: 0.5;
  }
  .reboot {
    position: absolute;
    top: 45%;
    left: 25%;
    width: 200px;
    height: 100px;
    margin: 0 auto;
    background-color: aqua;
    border-radius: 20px;
    font-style: italic;
    font-size: 2rem;
  }
  .rebootInvis {
    display: none;
  }
  .answer {
    cursor: pointer;
    &:hover {
      background-color: rgb(0, 114, 245);
    }
  }
</style>