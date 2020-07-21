<template>
  <div class="br-10 p-4 border">
    <div class="font-sm mb-5 pb-5 border-bottom">
      Skill Assessment Test: Frontend Developement
    </div>

    <VAsync :task="fetchQuestion" >
      <template v-slot="{  value: question }">
        <div class="row no-gutters align-items-center justify-content-between mb-2">
          <div class="flex-1 pr-4">
            <div class="font-md bold">
              {{ question.description }}
            </div>
          </div>
          <div class="s-60x60 border b-pink all-center round">
            <div class="t-align-c pink">
              <h2 class="bold">30</h2>
              <div class="font-sm">SEC</div>
            </div>
          </div>
        </div>

        <Choice
          :choice="choice"
          :index="index"
          v-for="(choice, index) in question.choices"
          :onSelect="(choiceId) => submitQuestion.run(question.id, choiceId)"
          :submissionResponse="submissionResponse"
          :key="choice.id" />

          <div class="row no-gutters justify-content-between align-items-center mt-5">
              <div class="col-sm-8 col-6">
                  <div class="med-grey font-sm"> {{questionsRemaining}} Questions Remaining</div>
                  <div class="mt-4">
                      <progress value="currentIndex" max="total"></progress>
                  </div>
              </div>
          </div>

      </template>

    </VAsync>
  </div>
</template>

<script>
import VAsync from '~/components/Base/VAsync'
import Choice from './Choice'

export default {
  name: 'Question',
  props: {
    questionId: {
      type: String,
      required: true
    },
    switchToNextQuestion: Function,
    userResponses: Array,
     total: Number,
     currentIndex: Number
  },
  components: {
    Choice,
    VAsync
  },
  data () {
    return {
      question: null,
      submissionResponse: null
    }
  },
  methods: {
    afterSelectClass(choiceId) {
      if (!this.submissionResponse)
        return ''
      const {correctlyAnswered, incorrectlyAnswered } = this.submissionResponse
      if (correctlyAnswered.includes(choiceId)) {
        return 'bg-green white'
      } else if(incorrectlyAnswered.includes(choiceId)) {
        return 'bg-red white'
      } else {
        return ''
      }
    }
  },
  computed: {
    isCorrectlyAnswered () {
      return this.submissionResponse.correctlyAnswered.length > 0
    },
    isIncorrectlyAnswered () {
      return !this.isCorrectlyAnswered
    },
    questionsRemaining () {
      return this.total - this.currentIndex
    }
  },
  tasks (t) {
    return {
      fetchQuestion: t(function * () {
        const response = yield this.$axios.get(`/questions/${this.questionId}`, {
          params: {
            include: 'choices'
          }
        })

        return this.$jsonApiStore.sync(response.data)
      }).runWith('questionId'),
      submitQuestion: t(function *(questionId, choiceId) {
        // if (this.submissionResponse)
        //   return (void 0)

        const { data } = yield this.$axios.post(`/questions/${questionId}/submit?showAnswers=true`, {
          markedChoices: [choiceId]
        })

        this.submissionResponse = data
        this.userResponses.push({id: questionId, markedChoices:[choiceId]})
        window.setTimeout(this.switchToNextQuestion, 1000)
      })
    }
  }
}
</script>