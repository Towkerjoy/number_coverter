<template>
  <div id="app">
    <div class="container">
        <!-- Header Section-->
        <header id="title">
            <h1>Number and Text Converter</h1>
        </header>
        <!-- Main Contain Section -->
        <main id="main">
            <!-- Number System Convertion -->
            <section id="number-convertion">
                <h2>Number System Convertion</h2>
                <div class="box">
                    <div class="input-group">
                        <label for="">Decimal</label>
                        <input type="text" v-model="numbers.decimal">
                    </div>
                    <div class="input-group">
                        <label for="">Binary</label>
                        <input type="text" v-model="numbers.binary">
                    </div>
                    <div class="input-group">
                        <label for="">Octal</label>
                        <input type="text" v-model="numbers.octal">
                    </div>
                    <div class="input-group">
                        <label for="">Hexadecimal</label>
                        <input type="text" v-model="numbers.hexadecimal">
                    </div>
                    <div class="reset">
                        <p v-if="inValidNumber" style="color:red;">
                            Invalid Input
                        </p>
                        <button @click="resetNumbers">Reset</button>
                    </div>
                    <div class="one-rem-space"></div>
                </div>
            </section>
            <!-- Text System Convertion -->
            <section id="text-convertion">
                <h2>Text System Convertion</h2>
                <div class="box">
                    <div class="input-group">
                        <label for="">Input Your Text</label>
                        <textarea name="text-field" id="" cols="30" rows="10" v-model="lines.text"></textarea>
                    </div>
                    <div class="half-rem-space"></div>
                    <div class="input-group">
                        <label for="">Input Your Binary Number</label>
                        <textarea name="binary-field" id="" cols="30" rows="10" v-model="lines.binary"></textarea>
                    </div>
                    <div class="reset">
                        <button @click="resetlines">Reset</button>
                    </div>
                    <div class="one-rem-space"></div>
                </div>
            </section>
        </main>
    </div>
    
  </div>
</template>

<script>

require('@/assets/style/reset.css');
require('@/assets/style/fonts.css');
require('@/assets/style/main.css');

function isBinary(text){
    for (let t of text){
        if (t !== '0' && t !== '1') return false
    }
    return true
}
function isOctal(text){
    for (let t of text){
        if (t < '0' || t > '7') return false
    }
    return true
}
function isHex(text){
    for (let t of text){
        if (
            (t < '0' || t > '9') &&
            (t < 'A' || t > 'F') &&
            (t < 'a' || t > "f")
        ){
            return false
        }
    }
    return true
}

function textToBinary(text){
    const charCondeArray = []
    for (let i in text){
        charCondeArray.push(text.charCodeAt(i))
    }
    const binaryArray = charCondeArray.map(char=>{
        return char.toString(2)
    })
    return binaryArray;
    
}

function binaryToText(codeArray){
    let text = ''
    for (let code of codeArray){
        const char = String.fromCharCode(parseInt(code, 2));
        text = text.concat(char);
    }
    return text;
}

export default {
  name: 'App',
  created(){
      document.title = 'Number Converter | Vue | Joy'
  },
  data(){
      return{
          numbers:{
             decimal: 0,
             binary: 0,
             octal: 0,
             hexadecimal: 0,
            },
            lines:{
                text:'',
                binary:''
            },
            inValidNumber : false,
            inValiLines: false
        };
    },
    methods:{
        resetNumbers(){
            this.numbers= {
                decimal: 0,
             binary: 0,
             octal: 0,
             hexadecimal: 0,
            };
            this.inValidNumber = false
        },
        resetlines(){
            this.lines= {
            text: '',
             binary: '',
            };
            this.inValidNumber = false
        },
    },
    watch:{
        'numbers.decimal': function(value){
            this.numbers.decimal = parseInt(value) || 0;
            this.numbers.binary = value.toString(2);
            this.numbers.octal = value.toString(8);
            this.numbers.hexadecimal = value.toString(16);
        },
        'numbers.binary': function(value){
            let decimal = parseInt(value, 2);

            if (!isBinary(value)){
                this.inValidNumber = true;
            }else {
                this.inValidNumber = false;
            }
            this.numbers.decimal = decimal;
            this.numbers.binary = value || 0;
            this.numbers.octal = decimal.toString(8);
            this.numbers.hexadecimal = decimal.toString(16);
        },
        'numbers.octal': function(value){
            let decimal = parseInt(`0${value}`, 8)|| 0;
            if(!isOctal(value)){
                this.inValidNumber = true;
            }else {
                this.inValidNumber = false;
            }
           this.numbers.decimal = decimal;
           this.numbers.binary = decimal.toString(2)|| 0;
           this.numbers.octal = value || 0;
           this.numbers.hexadecimal = decimal.toString(16);
        },
        'numbers.hexadecimal': function(value){
            let decimal = parseInt(`0x${value}`, 16)|| 0;
            if(!isHex(value)){
                this.inValidNumber = true;
            }else {
                this.inValidNumber = false;
            }
           this.numbers.decimal = decimal;
           this.numbers.binary = decimal.toString(2)|| 0;
           this.numbers.octal = decimal.toString(8);
           this.numbers.hexadecimal = value || 0;
        },
        'lines.text':function(value){
            if(value.length===0){
                this.lines.binary = '';
            }else {
                const codeArray = textToBinary(value);
                this.lines.binary = codeArray.join(' ');
            }
        },
        'lines.binary': function(value){
            if(value.length === 0){
                this.lines.text = '';
            }else{
                const codeArray = value.split(' ');
                const text = binaryToText(codeArray);
                this.lines.text = text;
            }
        }, 
    },
};
</script>


