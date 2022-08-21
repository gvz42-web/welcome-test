<template>
  <Transition name="slide">
    <div class="screen" v-if="show">
      <form>
        <h2 class="question">{{ question.q }}</h2>
        <div class="answers">
          <div class="option" v-for="answer in question.a" v-bind:key="answer.score" :class="current == answer.score ? 'active' : ''">
            <input type="radio" :id="answer.name" :value=answer.score v-model="current" @click="showSubmit = true"/>
            <label :for="answer.name">{{ answer.title }}</label>
          </div>
        </div>
        <button class="submit" @click="next" v-bind:class="{show: showSubmit}">
          Ответить
        </button>
      </form>
    </div>
  </Transition>
</template>

<script>
export default {
  data () {
    return {
      show: true,
      showSubmit: false,
      number: 0,
      current: '',
      score: {
        chill: 0,
        sea: 0,
        forest: 0,
        energy: 0,
        loneliness: 0
      },
      questions: [
        {
          q: 'Море или лес?',
          a: [
            {
              title: 'Море',
              score: 'sea',
              name: 'sea'
            },
            {
              title: 'Лес',
              score: 'forest',
              name: 'forest'
            }
          ]
        },
        {
          q: 'Кофе или чай?',
          a: [
            {
              title: 'Кофе',
              score: 'energy',
              name: 'coffee'
            },
            {
              title: 'Чай',
              score: 'chill',
              name: 'tea'
            }
          ]
        },
        {
          q: 'Как ты лучше провел вечер субботы?',
          a: [
            {
              title: 'Прогулка с друзьями',
              score: 'energy',
              name: 'walkWFriends'
            },
            {
              title: 'Посидеть дома с книжкой или сериалами',
              score: 'chill',
              name: 'stayHome'
            },
            {
              title: 'Погулять в одиночестве',
              score: 'loneliness',
              name: 'alone'
            }
          ]
        }
      ]
    }
  },
  computed: {
    question () {
      return this.questions[this.number]
    }
  },
  methods: {
    next (event) {
      event.preventDefault()
      if (this.number + 1 < this.questions.length) {
        setTimeout(() => {
          this.show = !this.show
        }, 600)
        this.number += 1
        this.show = !this.show
        this.showSubmit = false
        this.score[this.current]++
        this.current = ''
      } else {
        console.log(this.score)
        let res = ''
        for (const el in this.score) {
          res += this.score[el]
          console.log(this.score[el])
        }
        console.log(res)
        this.$router.push({ name: 'result', params: { city: JSON.stringify(this.score) } })
      }
    }
  }
}
</script>

<style>
.screen{
  font-family: Arial;
  background-color: rgba(0,0,0,0.5);
  backdrop-filter: blur(10px);
  border-radius: 20px;
  max-width: 600px;
  max-height: 500px;
  width: 100%;
  height: 100%;
  padding: 40px 30px;
  color: white;
}

.slide-leave-active,
.slide-enter-active {
  transition: 0.8s;
}
.slide-enter {
  transform: translate(150%, 150%) rotate(45deg);
}
.slide-leave-to {
  transform: translate(0, -150%) rotate(-45deg);
}

form {
  display: flex;
  height: 100%;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
}
.question {
  font-weight: 700;
}
.answers {
  width: 100%;
}
.option {
  width: 100%;
  border: 2px solid #fff;
  padding: 10px 20px;
  border-radius: 20px;
  margin-bottom: 10px;
  transition: all 0.4s;
}
label{
  display: block;
  width: 100%;
  height: 100%;
}
input {
  display: none;
}

.active {
  background-color: #fff;
  color: #000;
}

.submit {
  visibility: hidden;
  outline: none;
  border: none;
  border-radius: 20px;
  padding: 10px 30px;
  font-weight: 600;
  transition: all 0.4s;
}
.show {
  visibility: initial;
}
.submit:active {
  background-color: #000;
  color: white;
}
</style>
