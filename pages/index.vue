<template>
    <div class="bg-gradient-to-tr from-green-400 to-emerald-200 shadow-lg border-2 rounded-3xl container mx-auto max-w-xl translate-y-[30vh]">
      <div class="py-5 px-10">
        <h1 class="text-center font-bold text-2xl">Type in what you want to hear</h1> <br>
        <input id="text" class="p-5 min-w-full text-center border-[5px] rounded-xl bg-green-100 border-green-100 focus:border-green-300 focus:outline-none placeholder:text-gray-500" type="text" placeholder="Say something!" v-model="text"><br>
        <div class="flex justify-evenly">
          <div>
            <select name="lang" id="lang" class="mt-10 mx-auto border-2">
              <option value="en">EN</option>
              <option value="id">ID</option>
              <option value="es">ES</option>
            </select>
            <h2 class="text-center font-semibold">IN</h2>
          </div>
          <div>
            <h2 class="mt-10 font-semibold">──→ TO ──→</h2>
          </div>
          <div>
            <select name="toLang" id="toLang" class="mt-10 mx-auto border-2">
              <option value="en">EN</option>
              <option value="id">ID</option>
              <option value="es">ES</option>
            </select>
          </div>
        </div><br>
        <div class="flex justify-evenly pt-10">
          <button type="submit" @click="makeSound()" class="font-semibold">Speak!</button>
          <button type="submit" @click="translateText()" class="font-semibold">Translate!</button>
          <button type="submit" @click="translateSpeak()" class="font-semibold">Translate and Speak!</button>
        </div>
        <section id="translate" class="text-center pt-6 pb-2"></section>
      </div>
    </div>
</template>

<script>

import Speech from 'speak-tts'
import translate from 'translate'
import fs from 'fs'

  export default {
    data() {
      return {
        text: '',
      }
    },
    methods: {
      async makeSound() {
        const say = new Speech()

        say.speak({
          text: this.text
        })
      },
      async translateText() {
        const toLang = document.querySelector("#toLang").value
        console.log(toLang)

        if (this.text == '')
        {
          document.querySelector('#translate').innerHTML = ''
        }

        const trans = await translate(this.text, toLang)
        document.querySelector('#translate').innerHTML = trans
      },
      async translateSpeak() {

        const utterance = new SpeechSynthesisUtterance();

        const fromLang = document.querySelector("#lang").value
        const toLang = document.querySelector("#toLang").value
        let fromLangv2 = ''
        let toLangv2 = ''

        if (fromLang != 'en') {
          let fromLangv2 = fromLang +"-"+ fromLang.toUpperCase()
          console.log(fromLangv2)
        }
        else
        {
          fromLangv2 = 'en-US'
        }
        
        if (toLang != 'en') {
          toLangv2 = toLang +"-"+ toLang.toUpperCase()
          console.log(toLangv2)
        }
        else
        {
          toLangv2 = 'en-US'
        }

        console.log(fromLangv2, toLangv2)

        // translate.from = fromLang

        if (this.text == '')
        {
          document.querySelector('#translate').innerHTML = ''
        }

        const trans = await translate(this.text, { from: fromLang, to: toLang })

        document.querySelector('#translate').innerHTML = trans

        console.log(trans)

        utterance.text = trans

        utterance.lang = toLangv2

        window.speechSynthesis.speak(utterance);
      }
    },
  }
</script>
