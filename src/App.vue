<template>
  <div>
    <template v-if="this.question">

      <h1 v-html="this.question"></h1>

      <template v-for="(answer, index) in this.answers" :key="index">
        <input :disabled="this.answerSubmitted" type="radio" name="options" :value="answer" v-model="this.chosen_answer">
        <label v-html="answer"></label><br>
      </template>

      <button @click="this.submitAnswer()" class="send" type="button">Send</button>
      <section v-if="showAnswer">
        <h2>A resposta é {{ correctAnswer }}</h2>
      </section>
    </template>
  </div>
</template>

<script>
export default{
  name: 'App',

  data(){
    return{
      question: undefined,
      incorrectAnswers: undefined,
      correctAnswer: undefined,
      chosen_answer: undefined,
      answerSubmitted:false ,
    }
  },

  computed:{
    //faz com que o incorrectAnswer e o correctAnswer seja apenas um array , pois os 2 sao arrays separados
    answers(){
      //pega os resultados do incorrectAnswers e cria um novo array 
      var answers = [...this.incorrectAnswers];
      //splice define a posicao da resposta definindo a posicao do array e o math.random * 4 é pra definir qual das posicoes ele vai ficar entre 1 e 4
      answers.splice(Math.random() * 4, 0, this.correctAnswer); 
      return answers;
    }
  },
  methods:{
    submitAnswer(){
      this.showAnswer = true;
      if(!this.chosen_answer){
        alert('Pick one of the options!');
      }else{
        this.answerSubmitted = true;
        if(this.chosen_answer == this.correctAnswer){
          alert('You got it right!');
        }else{
          alert('You got it wrong!');
        }
      }
    }
  },
  //Link para fazer as requisições da API
  created(){
    this.axios.get('https://opentdb.com/api.php?amount=1')
    .then((response) =>{
      //Pegar as informaçoes do objeto [0]
      //variavel = requisição HTTP.dados.resultados[objeto]. a propriedade que o objeto tiver
      this.question = response.data.results[0].question;
      this.incorrectAnswers = response.data.results[0].incorrect_answers;
      this.correctAnswer = response.data.results[0].correct_answer;
      console.log(response.data.results[0]);
    })
  }
}
</script>

<style lang="scss">
*{
  background-color: rgb(59, 59, 59);
}
#app {
  font-family: Avenir, Arial, Helvetica, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #ffffff;
  margin:60px auto;
  max-width: 960px;

  input[type=radio]{
    margin:12px 4px;
  }
  h2{
    color: green;
  }
  button.send{
    margin-top: 12px;
    height:40px;
    min-width: 120px;
    padding: 0 16px;
    background-color: #1867c0;
    border:1px solid #1867c0;
    cursor: pointer;
  }
}
</style>