<template>
<div class="row">
  <div class="col-md-6">
    <form class="needs-validation">
      <div class="row form-group">
          <div class="col-6">
            <label for="baseWordField"><b>Base word</b></label>
            <input  v-model="baseWord"
                    @input="makePassword"
                    class="form-control" 
                    aria-describedby="baseWordHelp"
                    pattern="^[a-zA-Z]+$"
                    type="text"
                    id="baseWordField"
                    >
            <small id="baseWordHelp" class="form-text text-muted">Maybe pet name, parent name or your's favorite color.</small>
          </div>          
          <div class="col-6">
              <label><b>Special character</b></label>
              <select class="custom-select d-block w-100" 
                      v-model="specialCharSelected" 
                      @change="makePassword">
                <option 
                  v-for="(option, index) in specialChar"
                  :key="index" 
                  :value="option.c"
                  >{{ option.c }} {{ option.d }}
                </option>            
              </select>
          </div>        
      </div>
      <div class="row">
            <div class="col-12 form-group">Passwords for your websites</div>
      </div>
      <div v-for="(options, index) in passWords" 
            :key="index">
      <div  class="form-group row mb-0">
                <label class="col-sm-4 col-form-label">
                  <b>{{ options.s }}</b><br>
                </label>
                <div class="input-group col-sm-8">
                  <input :value="options.v" 
                          readonly 
                          class="form-control"
                          >
                  <div class="input-group-append">                               
                  <button type="button" 
                          class="btn btn-secondary btn-info" 
                          title="Remove" 
                          @click="delPassword(options)" 
                          v-if="passWords.length >= 3 ? 'hidden' : ''">Delete
                  </button>
                </div>
                </div>
       </div>
       <div class="row">
         <div class="col-sm-8  offset-sm-4"><small>Entropy: {{ options.e }} bits.</small></div>            
      </div>
      </div>
      <div  class="form-group row" 
            v-if="passWords.length <= 10 ? 'hidden' : ''">
            <label class="col-sm-4 col-form-label">
                  <b>New website:</b>
                </label>
          <div class="input-group col-sm-8">
                  <input  class="form-control" 
                          placeholder="twitter" 
                          v-model="newPassword">      
              <div class="input-group-append">
                      <button type="button" 
                              class="btn btn-secondary btn-success" 
                              title="Add" 
                              @click="addPassword">Insert
                      </button>
              </div>
          </div>
      </div>        
    </form>
  </div>
  <div class="col-md-6 card">
        <div class="card-body">
          <h2 class="card-title">Intro</h2>
          <p class="card-text">
            The <b>base word</b> is the key to the password, this should be your secret. 
            <b>Sign up word</b> is the title of the page you are logging in to (gmail, twitter, ...).
            <a href="https://en.wikipedia.org/wiki/Leet" title="Leet, also known as eleet or leetspeak." target="_blank">L33t</a> numbers are basically letters, numbers are similar to letters.<br>
            <span v-for="(char, index) in charLeet" :key="index" class="small">
             {{ char.c }} = {{ char.v }}<span v-if="(index+1 >= charLeet.length) ? '' : 'hidden'">,</span></span>
          </p>
          <h2>Entropy</h2>
          <p class="card-text">
          <a href="https://en.wikipedia.org/wiki/Password_strength#Entropy_as_a_measure_of_password_strength" title="Entropy as a measure of password strength" target="_blank">Entropy</a> is used to check the quality of the password.
          Password with an entropy of 42 bits would require 2<sup>42</sup> (4,398,046,511,104) attempts to exhaust all possibilities during a 
          <a href="https://en.wikipedia.org/wiki/Brute-force_attack" title="Brute-force attack" target="_blank">brute force</a>.
          </p>
          <h2 class="card-title">How do we build a password?</h2>
          <ol>
            <li>In the base word and the Sign up word, we change the first letter to uppercase.</li>
            <li>We add a special character at the beginning and end of each word.</li>
            <li>Let's combine both words into one.</li>
            <li>We replace all lower case letters with l33t numbers.</li>
          </ol>
          <h2 class="card-title">Outro</h2>
           <p class="card-text">The best passwords, in addition to lowercase letters, contain:</p>
          <ul>
            <li>at least 8 letters,</li>
            <li>at least two uppercase letters,</li>
            <li>at least two numbers,</li>
            <li>at least two <a href="https://owasp.org/www-community/password-special-characters" target="_blank">special characters</a>.</li>
          </ul>
          <p class="card-text">
            Our password far exceeds these conditions. Examples are given below the generator. You can also add your own websites.<br>
            <small>P.S. I wrote this app for a friend Kristijan, who was hacked.</small>  
          </p>
         
        </div>
  </div>
</div>
</template>
<script>
import stringEntropy from 'fast-password-entropy'

export default {
  name: 'PassGenerator',
  data() {
    return {
      baseWord: 'Kristijan',
      specialChar:[
        {c:'', d:'none'},
        {c:'!', d:'Exclamation'},
        {c:'"', d:'Double quote'},
        {c:'#', d:'Number sign - hash'},
        {c:'$', d:'Dollar sign'},
        {c:'%', d:'Percent'},
        {c:"&", d:'Ampersand'},
        {c:'\'', d:'Backslash'},
        {c:'(', d:'Left parenthesis'},
        {c:')', d:'Right parenthesis'},
        {c:'*', d:'Asterisk'},
        {c:'+', d:'Plus'},
        {c:',', d:'Comma'},
        {c:'-', d:'Minus'},
        {c:'.', d:'Full stop'},
        {c:'/', d:'Slash'},
        {c:':', d:'Colon'},
        {c:';', d:'Semicolon'},
        {c:'<', d:'Less than'},
        {c:'=', d:'Equal sign'},
        {c:'>', d:'Greater than'},
        {c:'?', d:'Question mark'},
        {c:'@', d:'At sign'},
        {c:'[', d:'Left bracket'},
        {c:'\\', d:'Backslash'},
        {c:']', d:'Right bracket'},
        {c:'^', d:'Caret'},
        {c:'_', d:'Underscore'},
        {c:'`', d:'Grave accent - backtick'},
        {c:'{', d:'Left brace'},
        {c:'|', d:'Vertical bar'},
        {c:'}', d:'Right brace'},
        {c:'~', d:'Tilde'}
      ],
      specialCharSelected: '',
      passWords: [
            {s:'Gmail', v:'', e:''},
            {s:'Amazon', v:'', e:''}, 
            {s:'Instagram', v:'', e:''},
            {s:'Reddit', v:'', e:''},
            {s:'Homepc', v:'', e:''},
            {s:'Workpc', v:'', e:''}, 
            {s:'Wifi', v:'', e:''}
      ],
      newPassword: '',
      charLeet: [
            {c:'o', v:0},
            {c:'i', v:1},
            {c:'z', v:2},
            {c:'e', v:3},
            {c:'a', v:4},
            {c:'s', v:5},
            {c:'d', v:6},
            {c:'t', v:7},
            {c:'b', v:8},
            {c:'g', v:9}
      ]
    }
  },
  methods: {
    delPassword(o){
        this.passWords.splice(this.passWords.indexOf(o), 1)
    },
    addPassword(){
      if(this.newPassword != '') {
        this.passWords.push({s:this.newPassword, v:this.makeLeet(this.newPassword)})
        this.makePassword()
      } 
    },
    makeLeet(text) {
      text = text.replace(/\s/g, '')  // remove spaces
      text = text.charAt(0).toUpperCase() + text.slice(1) // first char to UPPERCASE
      // replace characters with l33t numbers
      this.charLeet.forEach(e => {
        text = text.replace(new RegExp(e.c, "g"), e.v)
      }) 
     return text;
    }, 
    // build passwords from object passWords
    makePassword() {
      this.passWords.forEach((option, index) => {
        this.$set(this.passWords[index], 'v', 
            this.specialCharSelected + 
            this.makeLeet(this.baseWord) + 
            this.specialCharSelected + 
            this.specialCharSelected + 
            this.makeLeet(option.s) + 
            this.specialCharSelected)
        this.$set(this.passWords[index], 'e', stringEntropy(this.passWords[index].v));
      });

      
    }
  },
  mounted() {
    // random special character
    this.specialCharSelected = this.specialChar[Math.floor(Math.random() * this.specialChar.length)].c,
    this.makePassword();    
  }
}
</script>
