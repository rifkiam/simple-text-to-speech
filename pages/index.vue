<template>
  <div class="bg-[url('assets/bg.jpg')] bg-cover min-h-screen bg-no-repeat">
    <div class="text-center h-0 font-bold text-3xl translate-y-[20vh]">Type in what you want to hear!</div><br>
    <div class="bg-gradient-to-tr from-blue-500 to-blue-300 border-10 md:rounded-3xl container mx-auto max-w-full md:max-w-xl translate-y-[25vh]">
      <div class="py-8 px-10">
        <!-- <h1 class="text-center font-bold text-2xl">Type in what you want to hear</h1> <br> -->
        <input id="text" class="p-5 min-w-full text-center border-[5px] rounded-xl bg-blue-300 border-blue-300 focus:border-blue-200 focus:outline-none placeholder:text-gray-500" type="text" placeholder="Say something!" v-model="text"><br>
        <div class="text-center font-medium h-0 text-xl mt-6">Choose your preference</div>
        <div class="flex justify-center space-x-4 mt-5">
          <div>
            <select name="lang" id="inLang" class="mt-10 mx-auto border-2 rounded-lg">
              <option value="en-US">EN</option>
              <option value="id-ID">ID</option>
              <option value="es-ES">ES</option>
              <option value="ja-JP">JP</option>
            </select>
          </div>
          <div>
            <h2 class="mt-10 font-semibold">TO</h2>
          </div>
          <div>
            <select name="toLang" id="toLang" class="mt-10 mx-auto border-2 rounded-lg">
              <option value="en-US">EN</option>
              <option value="id-ID">ID</option>
              <option value="es-ES">ES</option>
              <option value="ja-JP">JP</option>
            </select>
          </div>
        </div><br>
        <div class="flex justify-center space-x-4 pt-2">
          <button type="submit" @click="makeSound()" class=" bg-stone-100 shadow-2xl rounded-xl font-semibold px-3 py-2 hover:bg-slate-300 duration-250">Speak!</button>
          <button type="submit" @click="translateSpeak()" class="bg-stone-100 rounded-xl font-semibold px-3 py-2 hover:bg-slate-300 duration-250">Translate and Speak!</button>
          <button type="submit" @click="translateText()" class="bg-stone-100 shadow-2xl rounded-xl font-semibold px-3 py-2 hover:bg-slate-300 duration-250">Translate!</button>
        </div>
        <div class="mb-3">
          <section id="translate" :class="{ 'translate-y-[1vh]': true, 'bg-stone-100': viewTranslate, 'shadow-2xl': viewTranslate, 'border-[2px]': viewTranslate, 'rounded-xl': true, 'text-center': true , 'py-1': true, 'px-2': true }"></section>
          <section id="warning" :class="{ 'translate-y-[1vh]': true, 'bg-stone-100': viewWarning, 'shadow-2xl': viewWarning, 'border-[2px]': viewWarning, 'rounded-xl': true, 'text-center': true , 'py-1': true, 'px-2': true }"></section>
          <!-- <button @click="downloadAu" id="download" :class="{ 'translate-y-[1vh]': true, 'visible': viewDownload, 'text-transparent': !viewDownload, 'bg-stone-100': viewDownload, 'shadow-2xl': viewDownload, 'border-[2px]': viewDownload, 'rounded-xl': true, 'text-center': true , 'py-1': true, 'px-2': true }">Donwload</button> -->
          <!-- <audio id="result" controls></audio> -->
        </div>

      </div>
    </div>
  </div>
</template>

<script>

//import Speech from 'speak-tts'
import translate from 'translate';

  export default {
    data() {
      return {
        viewDownload: false,
        viewTranslate: false,
        viewWarning: false,
        text: '',
        textToProcess: '',
        utteranceToProcess: null,
        // audio: 
      }
    },
    methods: {
      async makeSound() { 
        this.viewWarning = false
        this.viewTranslate = false
        this.viewDownload = false

        document.querySelector('#warning').innerHTML = ''
        document.querySelector('#translate').innerHTML = ''
        // say.speak({
        //   text: this.text
        // })
        // say.speak({
        //   text: this.text
        // })

        if (document.querySelector('#text').value === '') {
          this.viewWarning = true
          document.querySelector('#warning').innerHTML = 'Input something in the field!'
        }
        else
        {
          const utterance = new SpeechSynthesisUtterance();

          this.textToProcess = this.text
          
          utterance.text = this.text
          utterance.lang = document.querySelector('#inLang').value

          this.utteranceToProcess = utterance


          window.speechSynthesis.speak(utterance);
          this.viewDownload = true
        }
      },
      async translateText() {
        this.viewWarning = false
        this.viewTranslate = false
        this.viewDownload = false
        // const toLang = document.querySelector("#toLang").value
        // console.log(toLang)
        document.querySelector('#warning').innerHTML = ''
        document.querySelector('#translate').innerHTML = ''

        // if (this.text == '')

        const fromLang = (document.querySelector("#inLang").value).slice(0, -3)
        const toLang = (document.querySelector("#toLang").value).slice(0, -3)
        if (fromLang === toLang)
        {
          this.viewWarning = true
          document.querySelector('#warning').innerHTML = 'The language in which you are trying to translate and the destination language is identical'
        }
        
        else
        {
          this.viewTranslate = true
          const trans = await translate(this.text, { from: fromLang, to: toLang })
          console.log(trans)
          document.querySelector('#translate').innerHTML = trans
        }
        // const trans = await translate(this.text, toLang)
        // document.querySelector('#translate').innerHTML = trans
      },
      async translateSpeak() {
        this.viewWarning = false
        this.viewTranslate = false
        this.viewDownload = false
        
        document.querySelector('#warning').innerHTML = ''
        document.querySelector('#translate').innerHTML = ''

        const utterance = new SpeechSynthesisUtterance();
        const synth = window.speechSynthesis;

        const fromLang = document.querySelector("#inLang").value
        const toLang = document.querySelector("#toLang").value

        console.log(fromLang, toLang)
        
        let fromLangv2 = fromLang.slice(0, -3)
        let toLangv2 = toLang.slice(0, -3)
        
        console.log(fromLangv2, toLangv2)
        
        if (fromLang === toLang)
        {
          this.viewWarning = true
          this.viewTranslate = false
          document.querySelector('#warning').innerHTML = 'The language in which you are trying to translate and the destination language is identical'
        }
        else
        {
          this.viewTranslate = true
          if (this.text == '')
          {
            document.querySelector('#translate').innerHTML = ''
          }
  
          const trans = await translate(this.text, { from: fromLangv2, to: toLangv2 })
  
          document.querySelector('#translate').innerHTML = trans          

          this.textToProcess = trans

          utterance.text = trans
  
          utterance.lang = toLang

          this.utteranceToProcess = utterance
  
          synth.speak(utterance);
          this.viewDownload = true
        }
      },
      // async downloadAu() {
      //   const utterance = new SpeechSynthesisUtterance(this.textToProcess);
      //   // console.log(this.textToProcess)

      //   window.speechSynthesis.speak(utterance);

      //   utterance.onend = () => {
      //     // Create a new Blob containing the speech audio data
      //     const blob = new Blob([new Uint8Array(utterance.audioBuffer)], { type: 'audio/wav' });

      //     // Create a download link for the audio file
      //     const downloadLink = document.createElement('a');
      //     downloadLink.href = URL.createObjectURL(blob);
      //     downloadLink.download = 'speech.wav';
      //     downloadLink.click();
      //   };
      // },
      async downloadAu() {
        const utterance = this.utteranceToProcess;
        const audio = document.createElement('audio');
        audio.controls = true;
        document.body.appendChild(audio);
        utterance.onend = function() {
          const blob = new Blob([new XMLSerializer().serializeToString(utterance)], { type: 'application/ssml+xml' });
          const url = URL.createObjectURL(blob);
          audio.src = url;
        };
        synth.speak(utterance);
      }
      // async downloadAudio() {
      //   this.viewWarning = false
      //   this.viewTranslate = false
        
      //   document.querySelector('#warning').innerHTML = ''
      //   document.querySelector('#translate').innerHTML = ''

      //   const utterance = new SpeechSynthesisUtterance();
      //   const synth = window.SpeechSynthesis;

      //   // const fromLang = document.querySelector("#lang").value
      //   const fromLang = document.querySelector("#inLang").value
      //   const toLang = document.querySelector("#toLang").value

      //   console.log(fromLang, toLang)
        
      //   let fromLangv2 = fromLang.slice(0, -3)
      //   let toLangv2 = toLang.slice(0, -3)
        
      //   console.log(fromLangv2, toLangv2)
        
      //   if (fromLang === toLang)
      //   {
      //     this.viewWarning = true
      //     this.viewTranslate = false
      //     document.querySelector('#warning').innerHTML = 'The language in which you are trying to translate and the destination language is identical'
      //   }
      //   else
      //   {
      //     this.viewTranslate = true
      //     if (this.text == '')
      //     {
      //       document.querySelector('#translate').innerHTML = ''
      //     }
  
      //     const trans = await translate(this.text, { from: fromLangv2, to: toLangv2 })
  
      //     document.querySelector('#translate').innerHTML = trans          

      //     utterance.text = trans
  
      //     utterance.lang = toLang
  
      //     window.speechSynthesis.speak(utterance);
      //   }
      // }
    },
  }
</script>