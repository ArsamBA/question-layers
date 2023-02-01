<template>
   <div class="question-wrapper" v-if="questionLayers">    
      <div class="first-question" v-if="shownQuestions">
         <h1 class="question-title">{{ shownQuestions.q }}</h1>
         <div v-if="shownQuestions.type === 'radio'">
            <div class="question-answers radio-question-wrapper" v-for="(answer, index) in shownQuestions.a" :key="index">
               <input type="radio" :id="answer" name="question" :value="answer" @click="radioQuestionClicked">
               <label :for="answer">{{ answer }}</label>
            </div>
         </div>
         <div v-if="shownQuestions.type === 'checkbox'">
            <div class="question-answers radio-question-wrapper" v-for="(answer, index) in shownQuestions.a" :key="index">
               <input type="checkbox" :id="answer" :name="answer" :value="answer" ref="checkbox">
               <label :for="answer">{{ answer }}</label>
            </div>
         </div>
         <div v-if="shownQuestions.type === 'text'">
            <div class="question-answers" v-for="(answer, index) in shownQuestions.a" :key="index">
               <input type="text" :id="answer" :name="answer" :value="answer">
               <label :for="answer">{{ answer }}</label>
            </div>
         </div>
         <div v-if="shownQuestions.type === 'number'">
            <div class="question-answers" v-for="(answer, index) in shownQuestions.a" :key="index">
               <input type="number" :id="answer" :name="answer" :value="answer">
               <label :for="answer">{{ answer }}</label>
            </div>
         </div>
         <div v-if="shownQuestions.type === 'password'">
            <div class="question-answers" v-for="(answer, index) in shownQuestions.a" :key="index">
               <input type="password" :id="answer" :name="answer" :value="answer">
               <label :for="answer">{{ answer }}</label>
            </div>
         </div>
         <div v-if="shownQuestions.type === 'checkbox'">
            <input class="next-button" type="button" :id="shownQuestions.button" :name="shownQuestions.button" value="next" @click="checkboxAnswerClicked" />
         </div>
      </div>
   </div>
</template>

<script>
export default {
   data() {
      return {
         shownQuestions: null,
      }
   },
   props:{
      questionLayers: Array,
   },
   mounted() {
      const shownQuestions = this.questionLayers[0][0]
      console.log("🚀 ~ file: index.vue:39 ~ mounted ~ shownQuestions", shownQuestions)
      this.shownQuestions = shownQuestions
      
      const firstLayer = this.questionLayers[0]
      console.log("🚀 ~ file: index.vue:30 ~ mounted ~ firstLayer", firstLayer)
      if (firstLayer.length != 1){
         this.questionLayers = null
      } else {

      }
   },
   methods: {
      radioQuestionClicked(e){
         const value = e.target.value
         console.log("🚀 ~ file: index.vue:45 ~ answerClicked ~ value", value)
         
         const secondQuestionScenario = this.questionLayers[1]
         console.log("🚀 ~ file: index.vue:46 ~ answerClicked ~ secondQuestionScenario", secondQuestionScenario)
         
         const answer1 = secondQuestionScenario.at(0)
         console.log("🚀 ~ file: index.vue:56 ~ radioQuestionClicked ~ answer1", answer1)
         
         const answer2 = secondQuestionScenario.at(1)
         console.log("🚀 ~ file: index.vue:59 ~ radioQuestionClicked ~ answer2", answer2)
         
         if(value === 'Answer 1'){
            this.shownQuestions = answer1
            console.log("🚀 ~ file: index.vue:51 ~ answerClicked ~ answer1", answer1)
         } else if(value === 'Answer 2') {
            this.shownQuestions = answer2  
            console.log("🚀 ~ file: index.vue:52 ~ answerClicked ~ answer2", answer2)
         }
      },

      checkboxAnswerClicked(){
         const markedCheckbox = document.querySelectorAll('input[type="checkbox"]:checked');
         
         for (let checkbox of markedCheckbox) {
            
            console.log(checkbox.value)
            
            const thirdQuestionScenario = this.questionLayers[2]
            console.log("🚀 ~ file: index.vue:72 ~ checkboxAnswerClicked ~ thirdQuestionScenario", thirdQuestionScenario)
            
            const text = thirdQuestionScenario.at(0)
            console.log("🚀 ~ file: index.vue:77 ~ checkboxAnswerClicked ~ text", text)            
            const typeText = thirdQuestionScenario.at(0).type
            console.log("🚀 ~ file: index.vue:101 ~ checkboxAnswerClicked ~ typeText", typeText)

            const number = thirdQuestionScenario.at(1)
            console.log("🚀 ~ file: index.vue:79 ~ checkboxAnswerClicked ~ number", number)
            const typeNumber = thirdQuestionScenario.at(1).type
            console.log("🚀 ~ file: index.vue:106 ~ checkboxAnswerClicked ~ typeNumber", typeNumber)

            const password = thirdQuestionScenario.at(2)
            console.log("🚀 ~ file: index.vue:81 ~ checkboxAnswerClicked ~ password", password)
            const typePassword = thirdQuestionScenario.at(2).type
            console.log("🚀 ~ file: index.vue:111 ~ checkboxAnswerClicked ~ typePassword", typePassword)

            if(typeText === 'text'){
               this.shownQuestions = text
               console.log("🚀 ~ file: index.vue:112 ~ checkboxAnswerClicked ~ shownQuestions", shownQuestions)
            } else if(typeNumber === 'number') {
               this.shownQuestions = number  
               console.log("🚀 ~ file: index.vue:115 ~ checkboxAnswerClicked ~ shownQuestions", shownQuestions)
            } else if(typePassword === 'password') {
               this.shownQuestions = password  
               console.log("🚀 ~ file: index.vue:118 ~ checkboxAnswerClicked ~ shownQuestions", shownQuestions)
            }
            
         }
      }
   },
}
</script>

<style lang="scss">
h1,
p,
label{
   font-family: Arial, Helvetica, sans-serif;
}

.question-wrapper {
   padding: 0px 10px;
}

.radio-question-wrapper {
   margin-top: 30px;
   font-size: 20px;
}

.checkbox-question-wrapper {
   margin-top: 30px;
   font-size: 20px;
}

.next-button {
   background: #272727;
   color: #fff;
   margin-top: 30px;
   border-radius: 5px;
   font-size: 18px;
   padding: 8px 40px;
}

.first-question-wrapper {
   width: 100%;
   display: flex;
   flex-direction: column;
   justify-content: center;
   align-items: flex-start;
}

.first-question-wrapper div{
   margin-top: 25px;
}
</style>