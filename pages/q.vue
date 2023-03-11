<template>
   <div class="question-layers">
      <h5 class="displaying-array">mainQuestionLayer: {{ mainQuestionLayer }}</h5>
      <div class="utilities" v-if="utilities">
         <h5 class="displaying-array">questionLayer: {{ questionLayer }}</h5>
         <h5 class="displaying-array">firstQuestionArray: {{ firstQuestionArray }}</h5>
         <h5 class="displaying-array">secondQuestionArray: {{ secondQuestionArray }}</h5>
      </div>
      <!-- first question -->
      <div v-if="firstBTN">
         <button class="add-first-button adding-answer-button margin-left" v-if="firstQuestion === null" @click="askToAddFirstQuestion">Add first Question</button>
      </div>
      <div class="first-question-wrapper" v-if="firstQuestion !== null">
         <!-- <h5 class="displaying-array">{{ questionLayer }}</h5> -->
         <!-- add new question / remove question -->
         <!--  for each question -->
         <h1>Adding first Question:</h1>
         <div class="add-question-wrapper">
            <button class="adding-answer-button" v-if="newQuestion === null" @click="AddFirstQuestion">{{ addTheQuestion }}</button>
            <div class="first-question-add-question" v-if="newQuestion !== null">
               <input class="question-input" type="text" v-model="newQuestion" placeholder="Please write the Question">
               <button class="confirm-button" @click="addQuestion">confirm</button>
            </div>
            <p class="title-error-message error-message">Please Add question</p>
         </div>

         <!-- selecting type -->
         <select class="select-type" v-model="questionLayer[0].question_type">
            <option disabled value="">Please select Question Type 🔽</option>
            <option>Radio</option>
            <option>Checkbox</option>
         </select>
         <p class="type-error-message error-message">Please Select your Question Type</p>

         <!-- for each answer -->
         <div class="first-question-add-answer" v-if="answerIsAllowed.includes(questionLayer[0].question_type)">
            <button class="adding-answer-button" v-if="newAnswer === null" @click="AddFirstAnswer">{{ addTheAnswer }}</button>
            <p class="answer-error-message error-message">Please Add at Least 2 Answer</p>
            <div class="add-answer-wrapper" v-if="newAnswer !== null">
               <input class="question-input" type="text" v-model="newAnswer" placeholder="Please write the Answer Options">
               <button class="confirm-button" @click="addAnswer">confirm</button>
            </div>
            <h3>Answers:</h3>
            <h4 class="display-options" v-for="(answer, index) in questionLayer[0].answers" :key="index">{{ answer }}</h4>
         </div>
         <button class="submit-button" @click="firstQuestionSubmit">Submit</button>
      </div>

      <!-- second question -->
      <div v-if="secondBTN">
         <button class="add-first-button adding-answer-button margin-left" v-if="secondQuestion === null" @click="askToAddSecondQuestion">Add second Question</button>
      </div>
      <div class="first-question-wrapper" v-if="secondQuestion !== null">
         <h1>Adding second Question:</h1>
         <div class="add-question-wrapper">
            <button class="adding-answer-button" v-if="newQuestion === null" @click="AddFirstQuestion">{{ addTheQuestion }}</button>
            <div class="first-question-add-question" v-if="newQuestion !== null">
               <input class="question-input" type="text" v-model="newQuestion" placeholder="Please write the Question">
               <button class="confirm-button" @click="addQuestion">confirm</button>
            </div>
            <p class="title-error-message error-message">Please Add question</p>
         </div>

         <!-- selecting type -->
         <select class="select-type" v-model="questionLayer[0].question_type">
            <option disabled value="">Please select Question Type 🔽</option>
            <option>Radio</option>
            <option>Checkbox</option>
         </select>
         <p class="type-error-message error-message">Please Select your Question Type</p>

         <!-- for each answer -->
         <div class="first-question-add-answer" v-if="answerIsAllowed.includes(questionLayer[0].question_type)">
            <button class="adding-answer-button" v-if="newAnswer === null" @click="AddFirstAnswer">{{ addTheAnswer }}</button>
            <p class="answer-error-message error-message">Please Add at Least 2 Answer</p>
            <div class="add-answer-wrapper" v-if="newAnswer !== null">
               <input class="question-input" type="text" v-model="newAnswer" placeholder="Please write the Answer Options">
               <button class="confirm-button" @click="addAnswer">confirm</button>
            </div>
            <h3>Answers:</h3>
            <h4 class="display-options" v-for="(answer, index) in questionLayer[0].answers" :key="index">{{ answer }}</h4>
         </div>
         <button class="submit-button" @click="secondQuestionSubmit">Submit</button>
      </div>

      <!-- first question preview -->
      <div class="first-question-preview" v-if="firstQuestionPreview">
         <h5 class="rendering-array">{{ newArray }}</h5>
         <div class="newArray-wrapper" v-for="(createdQuestion, index) in firstQuestionArray" :key="index">
            <div v-if="radioQuestionCreated && createdQuestion.question_type === 'Radio'">
               <!-- <h5 class="rendering-array">{{ firstQuestionArray }}</h5> -->
               <div class="rendered-question">
                  <h1 class="question-title">{{ index + 1 }}.</h1>
                  <h1 class="question-title">{{ createdQuestion.title }}</h1>
               </div>
               <button class="adding-answer-button" v-if="!editFirstQuestionsTitle" @click="editFirstQuestionsTitleButton">Edit Title</button>
               <div class="edit-first-question-title" v-if="editFirstQuestionsTitle" style="margin-top: 20px;">
                  <input class="question-input" v-model="createdQuestion.title" />
                  <button class="confirm-button" v-if="editFirstQuestionsTitle" @click="editedFirstQuestion">confirm</button>
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
               <button class="adding-answer-button" v-if="!editFirstQuestionsTitle" @click="editFirstQuestionsTitleButton">Edit Title</button>
               <div class="edit-first-question-title" v-if="editFirstQuestionsTitle" style="margin-top: 20px;">
                  <input class="question-input" v-model="createdQuestion.title" />
                  <button class="confirm-button" v-if="editFirstQuestionsTitle" @click="editedFirstQuestion">confirm</button>
               </div>
               <div class="question-answers checkbox-question-wrapper" v-for="answer in createdQuestion.answers" :key="answer">
                  <input type="checkbox" :id="answer" name="question" :value="answer">
                  <label :for="answer">{{ answer }}</label>
               </div>
            </div>
         </div>
         <div class="final-buttons">
            <button class="submit-button" @click="firstQuestionFinalSubmit">Next Question</button>
            <button class="submit-button" @click="firstQuestionEnd">End</button>
         </div>
      </div>

      <!-- second question preview -->
      <div class="first-question-preview" v-if="secondQuestionPreview">
         <!-- <h5 class="rendering-array">{{ newArray }}</h5> -->
         <div class="newArray-wrapper" v-for="(createdQuestion, index) in secondQuestionArray" :key="index">
            <div v-if="radioQuestionCreated && createdQuestion.question_type === 'Radio'">
               <div class="rendered-question">
                  <h1 class="question-title">{{ index + 1 }}.</h1>
                  <h1 class="question-title">{{ createdQuestion.title }}</h1>
               </div>
               <div class="question-answers radio-question-wrapper" v-for="answer in createdQuestion.answers" :key="answer">
                  <input type="radio" :id="answer" name="question" :value="answer">
                  <label :for="answer">{{ answer }}</label>
               </div>
               <div class="choosing-scenario">
                  <select name="question" id="previousQuestionAnswerSelect">
                     <option disabled selected>Choose an Answer: 🔽</option>
                     <option v-for="lastQuestionAnswer in firstQuestionArray[0].answers" :key="lastQuestionAnswer" :value="lastQuestionAnswer">{{ lastQuestionAnswer }}</option>
                  </select>
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
               <div class="choosing-scenario">
                  <select name="question" id="previousQuestionAnswerSelect">
                     <option disabled selected>Choose an Answer: 🔽</option>
                     <option v-for="lastQuestionAnswer in firstQuestionArray[0].answers" :key="lastQuestionAnswer" :value="lastQuestionAnswer">{{ lastQuestionAnswer }}</option>
                  </select>
               </div>
            </div>
         </div>
         <div class="final-buttons">
            <button class="submit-button" @click="secondQuestionFinalSubmit">Next Question</button>
            <button class="submit-button" @click="secondQuestionEnd">End</button>
         </div>
      </div>
      <h1 v-if="successMessage" class="Success-message">Your Questions Add to Your Website</h1>
   </div>
</template>
<script>
export default{
   data(){
      return {
         newQuestion: null,
         newAnswer: null,
         firstQuestion: null,
         firstQuestionPreview: false,
         editFirstQuestionsTitle: false,
         secondQuestion: null,
         secondQuestionPreview: false,
         firstBTN: true,
         secondBTN: false,
         addTheQuestion: 'Add Question',
         addTheAnswer: 'Add Answer',
         question: false,
         radioQuestionCreated: false,
         checkboxQuestionCreated: false,
         successMessage: false,
         utilities: true,
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
         mainQuestionLayer:[],
         firstQuestionArray: [],
         secondQuestionArray: [],
      }
   },
   mounted(){
      console.log("🚀 ~ file: q.vue:182 ~ firstQuestionSubmit ~ this.secondBTN", this.secondBTN)
   },
   methods: {
      AddFirstQuestion() {
         this.newQuestion = ''
      },
      AddFirstAnswer() {
         this.newAnswer = ''
      },
      askToAddFirstQuestion() {
         this.firstQuestion = ''
      },
      editFirstQuestionsTitleButton(){
         this.editFirstQuestionsTitle = true
      },
      editedFirstQuestion() {
         this.editFirstQuestionsTitle = false
      },
      askToAddSecondQuestion() {
         this.secondQuestion = ''
      },
      addQuestion(){
         if (this.newQuestion){
            console.log("🚀 ~ file: q.vue:86 ~ addQuestion ~ this.newQuestion", this.newQuestion)
            this.questionLayer[0].title = this.newQuestion
            console.log("🚀 ~ file: q.vue:84 ~ addQuestion ~ this.questionLayer", this.questionLayer[0])
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
      firstQuestionSubmit(){
         console.log("🚀 ~ file: q.vue:133 ~ firstQuestionSubmit ~ this.questionLayer", this.questionLayer)
         console.log("🚀 ~ file: q.vue:134 ~ firstQuestionSubmit ~ this.questionLayer[0]", this.questionLayer[0])
         
         if(this.questionLayer[0].question_type === 'Radio'){
            this.radioQuestionCreated = true
         } else if (this.questionLayer[0].question_type === 'Checkbox') {
            this.checkboxQuestionCreated = true
         }

         console.log('length', this.questionLayer[0].answers.length)

         if (this.questionLayer[0].title == ''){
            let error = document.querySelector('.title-error-message')
            error.style.display = 'initial'
            setTimeout(() => {
               error.style.display = 'none'
            }, "3000")
         }

         if (this.questionLayer[0].question_type == ''){
            let error = document.querySelector('.type-error-message')
            error.style.display = 'initial'
            setTimeout(() => {
               error.style.display = 'none'
            }, "3000")
         }
         
         if(this.questionLayer[0].answers.length < 2 && this.questionLayer[0].question_type != ''){
            let error = document.querySelector('.answer-error-message')
            error.style.display = 'initial'
            setTimeout(() => {
               error.style.display = 'none'
            }, "3000")
         }
         
         if(this.questionLayer[0].answers.length >= 2 && this.questionLayer[0].question_type != '' && this.questionLayer[0].title != '' && this.questionLayer[0].answers != ''){
            this.firstQuestionArray.push({... this.questionLayer[0]})
            this.questionLayer[0].title = '',
            this.questionLayer[0].question_type = '',
            this.questionLayer[0].answers = [],
            this.firstQuestion = null
            this.firstBTN = false;
            this.firstQuestionPreview = true;
         }


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

      firstQuestionFinalSubmit() {
         this.firstQuestionPreview = false
         this.secondBTN = true;
      },

      firstQuestionEnd() {
         this.firstQuestionPreview = false
         this.successMessage = true
         this.utilities = false
         this.mainQuestionLayer.push({... this.firstQuestionArray})
      },

      secondQuestionSubmit(){
         if(this.questionLayer[0].question_type === 'Radio'){
            this.radioQuestionCreated = true
         } else if (this.questionLayer[0].question_type === 'Checkbox') {
            this.checkboxQuestionCreated = true
         }

         if (this.questionLayer[0].title == ''){
            let error = document.querySelector('.title-error-message')
            error.style.display = 'initial'
            setTimeout(() => {
               error.style.display = 'none'
            }, "3000")
         }

         if (this.questionLayer[0].question_type == ''){
            let error = document.querySelector('.type-error-message')
            error.style.display = 'initial'
            setTimeout(() => {
               error.style.display = 'none'
            }, "3000")
         }
         
         if(this.questionLayer[0].answers.length < 2 && this.questionLayer[0].question_type != ''){
            let error = document.querySelector('.answer-error-message')
            error.style.display = 'initial'
            setTimeout(() => {
               error.style.display = 'none'
            }, "3000")
         }
         
         if(this.questionLayer[0].answers.length >= 2 && this.questionLayer[0].question_type != '' && this.questionLayer[0].title != '' && this.questionLayer[0].answers != ''){
            this.secondQuestionArray.push({... this.questionLayer[0]})
            this.questionLayer[0].title = '',
            this.questionLayer[0].question_type = '',
            this.questionLayer[0].answers = [],
            this.firstQuestion = null
            this.secondQuestionPreview = true;
         }


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

      secondQuestionFinalSubmit() {
         let select = document.querySelectorAll("#previousQuestionAnswerSelect");
         let value = select.options[select.selectedIndex].value;
         console.log("🚀 ~ file: q.vue:322 ~ secondQuestionFinalSubmit ~ value:", value)
      },

      secondQuestionEnd() {
         this.secondQuestionPreview = false
         this.secondBTN = false
         this.secondQuestion = null
         this.successMessage = true
         this.utilities = false
      },
   }
}

</script>

<style>
.question-layers{
   min-height: 58vh;
   display: flex;
   flex-direction: column;
}

.add-first-button{
   display: block;
}

button{
   cursor: pointer;
}

.displaying-array{
   margin-bottom: 10px;
}

.first-question-wrapper {
   padding: 0px 20px;
   margin-bottom: 20px;
}

.add-question-wrapper{
   margin-bottom: 30px;
}

.first-question-add-question{
   display: flex;
   flex-direction: row;
   align-items: center;
   justify-content: center;
}

.select-type{
   display: block;
   background: #1E1E1E;
   color: #fff;
   border-radius: 5px;
   padding: 10px 10px;

}

.first-question-preview {
   padding: 0px 20px;
   border-top: 1px solid #1E1E1E;
   margin-top: 30px;
   padding-top: 30px;
}

.display-options{
   margin-left: 50px;
}

.margin-left{
   margin-left: 20px;
}

.question-input {
   margin-right: 10px;
   width: 300px;
   height: 30px;
   border-radius: 5px;
   outline: none;
   border: 2px solid #1E1E1E;
   padding-left: 10px;
}

.adding-answer-button {
   margin-top: 20px;
   padding: 10px 15px;
   border-radius: 5px;
   background-color: #1E1E1E;
   color: #fff;
}  

.confirm-button {
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

.choosing-scenario{
   margin-top: 40px;
}

.choosing-scenario label {
   font-size: 20px;
   font-weight: bold;
   color: red;
}

.choosing-scenario select {
   padding: 10px 15px;
   border-radius: 5px;
   background-color: #1E1E1E;
   color: #fff;
}

.edit-first-question-title{
   display: flex;
   flex-direction: row;
}

.utilities {
   margin-left: 20px;
}

.error-message{
   color: red;
   display: none;
}

.final-buttons{
   display: flex;
   flex-direction: row;
}

.final-buttons button:last-child{
   background-color: red;
   margin-left: 15px;
}

.Success-message{
   color: #0bc633;
   margin-top: 100px;
   margin-left: 20px;
}
</style>