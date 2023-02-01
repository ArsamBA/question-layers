<template>
   <div>
      <div class="question-wrapper">
         <h5 class="displaying-array">{{ questionLayer }}</h5>
         <!-- add new question / remove question -->
         <!--  for each question -->
         <h1>Create your first Question:</h1>
         <button class="adding-answer-button" v-if="newQuestion === null" @click="askToAddQuestion">{{ addTheQuestion }}</button>
         <input class="question-input" v-if="newQuestion !== null" type="text" v-model="newQuestion" placeholder="Please write the Question">
         <p class="error title-error">please add question</p>
         <button class="confirm-button" v-if="newQuestion !== null" @click="addQuestion">confirm</button>
         <h3>Question:</h3>
         <h4 class="display-options">{{ questionLayer[0].title }}</h4>

         <!-- selecting type -->
         <select class="select-type" v-model="questionLayer[0].question_type">
            <option disabled value="">Please select Question Type 🔽</option>
            <option>Radio</option>
            <option>Checkbox</option>
         </select>
         <p class="error type-error">please choose type</p>
         <h3>Question Type:</h3>
         <h4 class="display-options">{{ questionLayer[0].question_type }}</h4>

         <!-- for each answer -->
         <div v-if="answerIsAllowed.includes(questionLayer[0].question_type)">
            <button class="adding-answer-button" v-if="newAnswer === null" @click="askToAddAnswer">{{ addTheAnswer }}</button>
            <input class="question-input" v-if="newAnswer !== null" type="text" v-model="newAnswer" placeholder="Please write the Answer Options">
            <button class="confirm-button" v-if="newAnswer !== null" @click="addAnswer">confirm</button>
            <p class="error answer-error">please add answer</p>
            <h3>Answers:</h3>
            <h4 class="display-options" v-for="(answer, index) in questionLayer[0].answers" :key="index">{{ answer }}</h4>
         </div>
         <button class="submit-button" @click="submit">Submit</button>
      </div>

      <div class="answer-wrapper">
         <h5 class="rendering-array">{{ newArray }}</h5>
         <div class="newArray-wrapper" v-for="(createdQuestion, index) in newArray" :key="index">
            <div v-if="radioQuestionCreated && createdQuestion.question_type === 'Radio'">
               <div class="rendered-question">
                  <h1 class="question-title">{{ index + 1 }}.</h1>
                  <h1 class="question-title">{{ createdQuestion.title }}</h1>
               </div>
               <div class="question-answers radio-question-wrapper" v-for="answer in createdQuestion.answers" :key="answer">
                  <input type="radio" :id="answer" name="question" :value="answer">
                  <label :for="answer">{{ answer }}</label>
               </div>
            </div>
            <div v-if="checkboxQuestionCreated && createdQuestion.question_type === 'Checkbox'">
               <div class="rendered-question">
                  <h1 class="question-title">{{ index + 1 }}.</h1>
                  <h1 class="question-title">{{ createdQuestion.title }}</h1>
               </div>
               <div class="question-answers checkbox-question-wrapper" v-for="answer in createdQuestion.answers" :key="answer">
                  <input type="checkbox" :id="answer" name="question" :value="answer">
                  <label :for="answer">{{ answer }}</label>
               </div>
            </div>
         </div>
      </div>
   </div>
</template>
<script>
export default{
   data(){
      return {
         newQuestion: null,
         newAnswer: null,
         addTheQuestion: 'Add Question',
         addTheAnswer: 'Add Answer',
         question: false,
         radioQuestionCreated: false,
         checkboxQuestionCreated: false,
         answerIsAllowed : [
            'Radio',
            'Checkbox',
         ],
         questionLayer: [
            {  
               title: "",
               question_type: "",
               answers: []
            }
         ],
         newArray: [],
      }
   },
   methods: {
      askToAddQuestion() {
         this.newQuestion = ''
      },
      askToAddAnswer() {
         this.newAnswer = ''
      },
      addQuestion(){
         if (this.newQuestion){
            console.log("🚀 ~ file: q.vue:86 ~ addQuestion ~ this.newQuestion", this.newQuestion)
            this.questionLayer[0].title = this.newQuestion
            console.log("🚀 ~ file: q.vue:84 ~ addQuestion ~ this.questionLayer", this.questionLayer)
            this.newQuestion = null
         }

         if(this.questionLayer[0].title !== '') {
            this.addTheQuestion = 'Edit Question'
         } else {
            this.addTheQuestion = 'Add Question'
         }
      },
      addAnswer(){
         if (this.newAnswer != ''){
            this.questionLayer[0].answers.push(this.newAnswer)
            this.newAnswer = null
         }

         if(this.questionLayer[0].answers !== '') {
            this.addTheAnswer = 'Add Another Answer'
         } else {
            this.addTheAnswer = 'Add Answer'
         }
      },
      submit(){
         if(this.questionLayer[0].question_type === 'Radio'){
            this.radioQuestionCreated = true
         } else if (this.questionLayer[0].question_type === 'Checkbox') {
            this.checkboxQuestionCreated = true
         }
         
         
         if(this.questionLayer[0].question_type != '' && this.questionLayer[0].title != '' && this.questionLayer[0].answers != ''){
            this.newArray.push({... this.questionLayer[0]})
            this.questionLayer[0].title = '',
            this.questionLayer[0].question_type = '',
            this.questionLayer[0].answers = [],
            console.log("🚀 ~ file: q.vue:105 ~ submit ~ this.questionLayer", this.questionLayer)
            console.log("🚀 ~ file: q.vue:89 ~ submit ~ newArray", this.newArray)
         }

         // // initial error 
         // if(this.questionLayer[0].title === ''){
         //    document.querySelector('.title-error').style.display = 'initial'
         // } else if(this.questionLayer[0].question_type === ''){
         //    document.querySelector('.type-error').style.display = 'initial'         
         // } else if(this.questionLayer[0].answers === ''){
         //    document.querySelector('.answer-error').style.display = 'initial'
         // }

         // // hiding error
         // if (document.querySelector('.title-error').style.display = 'initial'){
         //    setTimeout(() => {         
         //       document.querySelector('.title-error').style.display = 'none'
         //    }, 2000)
         // }
         // if (document.querySelector('.type-error').style.display = 'initial'){
         //    setTimeout(() => {         
         //       document.querySelector('.title-error').style.display = 'none'
         //    }, 2000)
         // }
         // if (document.querySelector('.answer-error').style.display = 'initial'){
         //    setTimeout(() => {         
         //       document.querySelector('.title-error').style.display = 'none'
         //    }, 2000)
         // }


         // reseting add buttons
         if(this.questionLayer[0].title !== '') {
            this.addTheQuestion = 'Edit Question'
         } else {
            this.addTheQuestion = 'Add Question'
         }

         if(this.questionLayer[0].answers !== '') {
            this.addTheAnswer = 'Add Another Answer'
         } else {
            this.addTheAnswer = 'Add Answer'
         }
      },

   }
}

</script>

<style>
button{
   cursor: pointer;
   margin-bottom: 10px;
}

.displaying-array{
   margin-bottom: 30px;
}

.question-wrapper {
   padding: 0px 20px;
   border-bottom: 1px solid #1E1E1E;
   margin-bottom: 20px;
}

.select-type{
   display: block;
   margin: 50px 0px 20px;
   background: #1E1E1E;
   color: #fff;
   border-radius: 5px;
   padding: 10px 10px;

}

.answer-wrapper {
   padding: 0px 20px;
}

.display-options{
   margin-left: 50px;
}

.question-input {
   margin-top: 30px;
   width: 300px;
   height: 30px;
   border-radius: 5px;
   outline: none;
   border: 2px solid #1E1E1E;
   padding-left: 10px;
}

.adding-answer-button {
   margin-top: 40px;
   padding: 10px 15px;
   border-radius: 5px;
   background-color: #1E1E1E;
   color: #fff;
}  

.confirm-button {
   margin-top: 20px;
   padding: 6px 15px;
   border-radius: 5px;
   background-color: #0bb0c6;
   color: #fff;
   box-shadow: 0px 0px 4px #1e1e1e40;
} 

.confirm-button:active {
   box-shadow: 0px 0px 0px #1e1e1e;
} 

.submit-button {
   margin-top: 50px;
   padding: 10px 15px;
   border-radius: 5px;
   background-color: #0bc633;
   color: #fff;
   box-shadow: 0px 0px 4px #1e1e1e60;
   margin-bottom: 30px;
}

.submit-button:active {
   box-shadow: 0px 0px 0px #1e1e1e;
}  

.rendered-question{
   display: flex;
   flex-direction: row;
}

.newArray-wrapper{
   margin: 30px 0px 60px;
}

.error{
   color: red;
   font-size: 1em;
   display: none;
}
</style>