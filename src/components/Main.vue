<template>
  <div class="container jumbotron">
  <h1 class="display-4">Hızlı Yazma Yarışması</h1>
  <p class="lead">Klavye Hızını test et.</p>
  <div v-if="!isFinish" class="d-flex">
      <p>Doğru Sayınız:  {{ trueCount }}</p>
      <p class="ml-3">Yanlış Sayınız:  {{ falseCount }}</p>
  </div>

  <hr class="my-4">
  
  <div v-if="isFinish" class="alert alert-primary">
      <h1>Oyun Bitti</h1><br>
      <h4>Doğru Sayınız:   {{ trueCount }}</h4>
      <h4>Yanlış Sayınız:   {{ falseCount }}</h4>
  </div>
  
  <div v-else>
  <div class="card">
      <div class="card-body">
          <span 
            v-for="(word,key) in words.filter((data,index)=>index<20)" :key="key"
            v-bind:class="key!=0 || currentwordControl "
            class="words ml-2">
                {{word}}
          </span>
      </div>
  </div>

  <div class="card">
      <div class="card-body bg-secondary">
        <div class="input-group input-group-lg ">
        <input type="text" class="form-control" v-model="writingWord" >
        <div class="input-group-append" id="button-addon4">
            <button class="btn btn-light" disabled type="button">{{ timer }} sn</button>
            <button :disabled="isRunning" class="btn btn-light" type="button" @click="getWords">Yenile</button>
        </div>
</div>
      </div>
  </div>
  </div>

</div>
</template>

<script>
import wordList from '@/assets/words.json'

export default {
    data(){
        return{
            words:[],
            writingWord: null,
            isTrue: true,
            isRunning:false,
            isFinish:false,
            elementVisible:true,
            trueCount:0,
            falseCount:0,
            timer:60,
            interval:false,
            wordList:wordList    
        }
    },
    watch:{
        writingWord(val){
            if(!val || val=== '' ){
                this.writingWord = ''
                return
            }
            if(!this.isRunning) this.toggleTimer()
            const word = this.words[0].slice(0,val.length)
            const userWord = val.replace(' ','')

            this.isTrue = word === userWord


            if(val.indexOf(' ') !== -1){
                this.isTrue ? this.trueCount++ :this.falseCount++
                this.words.splice(0,1)
                this.writingWord = ''
            }
        }
    },
    computed:{
        currentwordControl(){
            return this.isTrue ? 'current-word' : 'current-word bg-danger'
        }
    },
    mounted(){
        this.getWords()
    },
    methods:{
        getWords(){
            this.words = this.wordList.sort(()=>Math.random()-0.5).splice(0,300)
        },
        toggleTimer (){
            this.isRunning = true
            this.interval = setInterval(this.timeProcess,1000)
        },
        timeProcess (){
            if(this.timer===0){
                clearInterval(this.interval)
                this.isFinish = true
                this.elementVisible = false

                return
            }
            this.timer--
        }
    }

}
</script>

<style>
    .words{
        font-size: 25px;
        font-weight: 400;
    }
    .current-word{
        background-color: grey;
        color:white;
        padding: 5px;
        border-radius: 5px;
    }

</style>