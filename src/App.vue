<template>
  <div id="app">
    <div class="game">
			<div class="game__left">
				<v-hanged-man :bad-attemps="badLetters.length"></v-hanged-man>
				<button class="button button__start" @click="init">Ricomincia</button>
			</div>
			<div class="game__right">
				<div class="settings">
					<button @click="settingsOpen = true" class="action action__settings" title="Configura il gioco">Settings</button>
					<div>
						Hai accumulato <img src="./assets/money.png" width="16" class="coin" /> <strong>{{ coins }}</strong>
					</div>
				</div>
				<div v-if="word2discover" class="words">
					<p>La parola da indovinare è:</p>
					<v-word :word="word2discover" :good-letters="goodLetters"></v-word>
					<form>
						<!-- input solution -->
						<!-- button solution -->
						<p class="tip">Sblocca i campi sopra e dai la soluzione adesso! Se sbagli, il gioco finisce.</p>
					</form>
					<template v-if="gameover">
						<p><strong>Oh no! HAI PERSO!</strong></p>
						<p>
							La parola era <strong>{{ word2discover }}</strong>
						</p>
					</template>
				</div>
				<div class="dashboard">
					<div class="consonants">
						<p>Prova con una consonante:</p>
						<form>
							<input
								v-model="letter2check"
								type="text"
								maxlength="1"
								placeholder="inserisci una consonante"
								class="input input__letter"
								pattern="[^aeiouAEIOU]"
								:disabled="totalAttemps === usedAttemps || gameover"
								required
							/>
							<button :disabled="totalAttemps === usedAttemps || gameover" class="action action__confirm" @click.prevent="confirm">Controlla</button>
						</form>
						<div class="tip tip__attemps">
							<p v-html="attempsMessage"></p>
						</div>
					</div>
					<div class="vocals">
						<p>oppure compra una vocale:</p>
						<ul>
							<li class="vocal">
								<!-- vocal -->
							</li>
						</ul>
						<!-- message tip -->
					</div>
				</div>
			</div>
		</div>
		<v-modal v-if="settingsOpen" @close-modal="settingsOpen = false">
			<form class="sentence" @submit.prevent="getWords">
				<label for="sentence">Configura il gioco</label>
				<textarea v-model="customText" id="sentence" placeholder="Copia qui testo da cui estrarre parole"></textarea>
				<button class="button button__start">Estrapola parole</button>
			</form>
		</v-modal>
  </div>
</template>

<script>
import HangedMan from './components/HangedMan.vue'
import WordToDiscover from './components/Word.vue'

export default {
  name: 'App',
  components: {
    'v-word': WordToDiscover,
    'v-hanged-man': HangedMan,
    'v-modal': () => import('./components/BaseModal.vue') // loaded asyncronusly
  },
  data: function() {
return {
    coins: '0 monete',
    words: ['stupendo', 'magnifico', 'meraviglioso', 'incredibile','fantastico'],
    word2discover: '',
    letter2check: '',
    goodLetters: [],
    badLetters: [],
    gameover: false,
    totalAttemps: 10,
    usedAttemps: 0,
    customText: '',
    settingsOpen: false
  };
},
  computed: {
    badAttemps() {
      return this.badLetters.length
    },
    attempsMessage() {
      return `Indovina la parola entro <strong>${this.totalAttemps - this.usedAttemps} tentativi</strong>`
    }
  },
  created() {
    this.init()
  },
  methods: {
    init() {
      // reset previous game
      this.goodLetters = []
      this.badLetters = []
      this.usedAttemps = 0
      this.gameover = false
      // choose another word
      const randNum = Math.floor(Math.random() * this.words.length)
      this.word2discover = this.words[randNum]
    },
    confirm() {
      this.usedAttemps++
      const letter = this.letter2check.toLowerCase()
      if (this.word2discover.includes(letter)) {
        if (!this.goodLetters.includes(letter)) this.goodLetters.push(letter)
      } else {
        this.badLetters.push(letter)
      }
      if (this.badLetters.length >= 6) this.gameover = true
      this.letter2check = ''
    },
    getWords() {
      this.words = this.customText
        .toLowerCase()
        .replace(/[^a-zA-Z]/gm, ' ')
        .split(' ')
        .filter(word => word.length > 7)
      this.settingsOpen = false
      this.init()
    }
  },
}
</script>

<style lang="scss">

// generic

* {
  box-sizing: border-box;
  font: inherit;
}

html,
body {
  margin: 0;
  padding: 0;
  height: 100%;
}
html {
  font-size: 10px;
}
body {
  font-family: 'Source Sans Pro', Arial, sans-serif;
  font-size: 1.4em;
  color: #292828;
}
p {
  margin: 0;
}
strong {
  font-weight: bold;
}
img {
  vertical-align: bottom;
  &.coin {
    display: inline-block;
    margin-right: 0.3rem;
  }
}



.game {
  padding: 1rem;
}
@media screen and (min-width: 800px) {
  .game {
    display: flex;
    max-width: 1024px;
    margin: 60px auto;
    &__left {
      display: flex;
      flex-direction: column;
    }
    &__right {
      margin-left: 2em;
      flex: 1 0 auto;
      flex-direction: column;
    }
  }
}
.button {
  border: none;
  background: none;
  height: 4rem;
  padding: 0;
  min-width: 230px;
  border-radius: 8px;
  color: white;
  text-transform: uppercase;
  font-size: 20px;
  font-weight: bold;
  margin: 1rem auto;
  cursor: pointer;
  &__start {
    width: 100%;
    background: #f4c34b url('assets/dices.png') no-repeat center right 10px;
  }
  &__end {
    background-color: #b8e986;
  }
}
.action {
  border: none;
  background: none;
  padding: 0;
  height: 22px;
  width: 22px;
  cursor: pointer;
  font-size: 0;
  &__settings {
    background: url('assets/settings.png') no-repeat center;
    background-size: cover;
  }
  &__confirm {
    vertical-align: middle;
    width: 4rem;
    height: 4rem;
    border-radius: 50%;
    position: absolute;
    margin-left: -2rem;
    background: #75befe url('assets/chevron.png') no-repeat center;
    &:disabled {
      background-color: #d6d6d6;
      cursor: default;
    }
  }
}
.settings {
  display: flex;
  justify-content: space-between;
}

.words {
  text-align: center;
  margin-top: 3em;
}

.dashboard {
  border-radius: 8px;
  background-color: #f6f6f6;
  text-align: center;
  margin-top: 4rem;
  padding: 2rem;
}

.tip {
  font-size: 1.2rem;
  text-align: center;
  color: #5a5a5a;
  &__attemps {
    margin-top: 0.5em;
  }
}

.consonants {
  p {
    margin-bottom: 1.5rem;
  }
}

.vocals {
  margin: 3rem 0 0;
  ul {
    margin: 1rem 0;
    padding: 0;
    list-style-type: none;
  }
  &--allowed {
    .vocal {
      cursor: pointer;
      background-color: #d0e9ff;
      color: #292828;
      &:hover {
        background-color: #75befe;
      }
    }
  }
}
.vocal {
  background-color: #d6d6d6;
  color: #a7a7a7;
  cursor: default;
  border-radius: 50%;
  width: 4rem;
  height: 4rem;
  line-height: 4rem;
  font-size: 2rem;
  text-align: center;
  text-transform: uppercase;
  display: inline-block;
  font-weight: bold;
  + .vocal {
    margin-left: 1.5rem;
  }
}

.letter,
.input {
  border: none;
  width: 4rem;
  height: 4rem;
  display: inline-block;
  background-color: white;
  text-transform: uppercase;
}
.input {
  &__letter {
    vertical-align: middle;
    border: dashed 1px #979797;
    background-color: #ffffff;
    padding: 0 2.5rem 0 2rem;
    width: 250px;
    text-align: center;
    &::placeholder {
      color: #b4b3b3;
      text-transform: none;
    }
    &:focus {
      border: solid 2px #75befe;
      border-radius: 4px;
      outline: none;
    }
    &:disabled {
      opacity: 0.5;
    }
  }
  &__word {
    border: dashed 1px #979797;
    background-color: #ffffff;
    padding: 0 2.5rem 0 2rem;
    width: 250px;
    display: block;
    margin: 0 auto;
    text-align: center;
    &::placeholder {
      color: #b4b3b3;
      text-transform: none;
    }
    &:focus {
      border: solid 2px #b8e986;
      border-radius: 4px;
      outline: none;
    }
  }
}

.letter::placeholder {
  text-transform: none;
}


.sentence {
  label {
    display: block;
    font-size: 1.2em;
    margin-bottom: 1rem;
    font-weight: 600;
  }
  textarea {
    width: 100%;
    height: 200px;
    border: dashed 1px #979797;
    background-color: #ffffff;
    padding: 1rem;
  }
}

@media screen and (max-width: 799px) {
  .hanged {
    margin: 0 auto;
    background-color: #f6f6f6;
    border-radius: 50%;
    display: block;
    width: 120px;
    height: 120px;
    padding: 1rem;
  }
  svg {
    width: 100px;
    height: 100px;
  }
}
</style>
