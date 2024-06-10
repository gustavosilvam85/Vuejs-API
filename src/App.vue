<template>
  <div>
    <h1 v-html="this.question"></h1>

    <input type="radio" name="options" value="True">
    <label>True</label><br>

    <input type="radio" name="options" value="False">
    <label>False</label><br>

    <button class="send" type="button">Send</button>
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