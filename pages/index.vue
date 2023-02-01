<template>
  <div>
    <Question :questionLayers="questionLayers" />
  </div>
</template>

<script>
export default {
  components:{
    Question: () => import('@/components/questionCreatorMechine/index.vue'),
  },
  data() {
    return {
      questionLayers: [
        [
          {
            q: 'This is the first layer question?',
            a: ['Answer 1', 'Answer 2'],
            type: 'radio',
            button: 'firstBtn',
            answerGiven: null,
            questionNumber: 'first'
          },
        ],
        [
          {
            q: 'This is the second layer question first question?',
            a: ['Text', 'Number', 'Password'],
            type: 'checkbox',
            answerGiven: 'answer1'
          },
          {
            q: 'This  is the second layer question second question?',
            a: ['Text', 'Number'],
            type: 'checkbox',
            answerGiven: 'answer2'
          }
        ],
        [
          {
            q: 'Please fill the text input.',
            type: 'text'
          },
          {
            q: 'Please fill the number input.',
            type: 'number'
          },
          {
            q: 'Please fill the password input.',
            type: 'password'
          }
        ]
      ]
    }
  }
}
</script>

<style>

</style>