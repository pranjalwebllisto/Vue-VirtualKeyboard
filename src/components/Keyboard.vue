<template >
  <div>


    <textarea id="txtBox" placeholder="Click here to start typing" class="form-control" v-model="value" cols="30"
      rows="10" v-on:keydown="keyPressed" @click="enableKeyboard"></textarea>
    <div class="container">


      <div class="virtual-keyboard" v-if="showKeyboard">
        <div class="rowZero">
          <input type="button" value="`" class="Backtick" id="Backtick" @click="pressSpacebar('`' , `Backtick`)"
            v-if="!symbols_enabled">
          <input type="button" value="~" class="Tilde" id="Tilde" v-if="symbols_enabled"
            @click="pressSpacebar(`~`,`Tilde`)">
          <input type="button" v-for="key in keys" :key="key" @click="press(key)" v-if="!symbols_enabled" :value="key"
            :id="key" ref="key">
          <input type="button" v-for="symbol in symbols" :key="symbol" @click="press(symbol)" v-if="symbols_enabled"
            :value="symbol" :id="symbol" ref="key">
          <input type="button" value="Backspace" class="Backspace" @click="pressSpacebar(`delete`,`Backspace`)"
            id="Backspace">
        </div>
        <div class="rowOne">
          <input type="button" id="Tab" value="Tab" @click="pressSpacebar(tab , `Tab`)">
          <input type="button" :value="letter" v-if="smallcase" v-for="letter in letters[0]" :key="letter" ref="key"
            @click="press(letter)" />
          <input type="button" :value="letter" v-if="!smallcase" v-for="letter in letters_capital[0]" :key="letter"
            ref="key" @click="press(letter)" />
          <input type="button" value="[" class="SquareBracketOpen" id="SquareBracketOpen"
            @click="pressSpacebar(`[` , `SquareBracketOpen`)" v-if="!symbols_enabled">
          <input type="button" value="{" class="ParanthesisOpen" id="ParanthesisOpen"
            @click="pressSpacebar(`{` , `ParanthesisOpen`)" v-if="symbols_enabled">
          <input type="button" value="]" class="SquareBracketClose" id="SquareBracketClose"
            @click="pressSpacebar(`]` , `SquareBracketClose`)" v-if="!symbols_enabled">
          <input type="button" value="}" class="ParanthesisClose" id="ParanthesisClose"
            @click="pressSpacebar(`}` , `ParanthesisClose`)" v-if="symbols_enabled">

          <input type="button" value="\" class="Backslash" id="Backslash" @click="pressSpacebar(`\\` , `Backslash`)"
            v-if="!symbols_enabled">
          <input type="button" value="|" class="Pipe" id="Pipe" @click="pressSpacebar(`|` , `Pipe`)"
            v-if="symbols_enabled">
        </div>
        <div class=" rowTwo">
          <input type="button" @click="toggleLetters()" id="Capslock" value="Capslock">
          <input type="button" :value="letter" v-if="smallcase" v-for="letter in letters[1]" :key="letter" ref="key"
            @click="press(letter)" />
          <input type="button" :value="letter" v-if="!smallcase" v-for="letter in letters_capital[1]" :key="letter"
            ref="key" @click="press(letter)" />



          <input type="button" value=";" class="Semicolon" id="Semicolon" @click="pressSpacebar(';' , `Semicolon`)"
            v-if="!symbols_enabled">
          <input type="button" value=":" class="Colon" id="Colon" @click="pressSpacebar(':' , `Colon`)"
            v-if="symbols_enabled">
          <input type="button" value="'" class="Singlequote" id="Singlequote"
            @click="pressSpacebar('\'' , `Singlequote`)" v-if="!symbols_enabled">
          <input type="button" :value="doublequote" class="Doublequote" id="Doublequote"
            @click="pressSpacebar(doublequote , `Doublequote`)" v-if="symbols_enabled">


          <input type="button" @click="pressSpacebar('\r\n' , `Enter`)" id="Enter" value="Enter">
        </div>
        <div class="rowThree">
          <input type="button" :valconsole.log(this.value)ue="letter" v-if="smallcase" v-for="letter in letters[2]"
            :key="letter" ref="key" @click="press(letter)" />
          <input type="button" :value="letter" v-if="!smallcase" v-for="letter in letters_capital[2]" :key="letter"
            ref="key" @click="press(letter)" />
          <input type="button" value="," class="Comma" id="Comma" @click="pressSpacebar(',' , `Comma`)"
            v-if="!symbols_enabled">
          <input type="button" value="<" class="Lessthan" id="Lessthan" @click="pressSpacebar('<' , `Lessthan`)"
            v-if="symbols_enabled">

          <input type="button" value="." class="Fullstop" id="Fullstop" @click="pressSpacebar(',' , `Fullstop`)"
            v-if="!symbols_enabled">
          <input type="button" value=">" class="Greaterthan" id="Greaterthan"
            @click="pressSpacebar('<' , `Greaterthan`)" v-if="symbols_enabled">


          <input type="button" value="/" class="Forwardslash" id="Forwardslash"
            @click="pressSpacebar('/' , `Forwardslash`)" v-if="!symbols_enabled">
          <input type="button" value="?" class="Questionmark" id="Questionmark"
            @click="pressSpacebar('?' , `Questionmark`)" v-if="symbols_enabled">
          <input type="button" @click="toggle()" id="Shift" value="Shift">
          <input type="button" @click="disableKeyboard(`Poweroff`)" id="Poweroff" value="Close Keyboard">
        </div>
        <div class="rowSpace spacebar">
          <input type="button" value="Spacebar" id="Space" @click="pressSpacebar(space , `Space`)">
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import "bootstrap/dist/css/bootstrap.min.css";
import _ from "lodash";

export default {
  props: [],
  data() {
    return {
      showKeyboard: false,

      smallcase: true,//denotes whether buttons are switched to lowercase 
      symbols_enabled: false, //denotes whether numeric buttons are to be switched with symbols
      value: "", //dyanmic value to be displayed in the input field
      space: " ", //to add single space when spacebar is clicked
      tab: "    ", //to add 4 spaces when tab is clicked
      keys: [1, 2, 3, 4, 5, 6, 7, 8, 9, 0], //array for numeric buttons
      doublequote: '\"',
      letters: [['q', 'w', 'e', 'r', 't', 'y', 'u', 'i', 'o', 'p'], ['a', 's', 'd', 'f', 'g', 'h', 'j', 'k', 'l'], ['z', 'x', 'c', 'v', 'b', 'n', 'm']],//lower case alphabet buttons
      letters_capital: [['Q', 'W', 'E', 'R', 'T', 'Y', 'U', 'I', 'O', 'P'], ['A', 'S', 'D', 'F', 'G', 'H', 'J', 'K', 'L'], ['Z', 'X', 'C', 'V', 'B', 'N', 'M']], //uppercase alphabet buttons
      symbols: ['!', '@', '#', '$', '%', '^', '&', '*', '(', ')'], //symbol buttons
      symbol_arr: ['[', ']', '{', '}', '\\', '|', '`', this.doublequote, "'", ";", ":", ",", "<", ".", ">", "/", "?"], //required to check if event key exist  in this array
      symbol_dict: { '[': "SquareBracketOpen", ']': "SquareBracketClose", '{': "ParanthesisOpen", '}': "ParanthesisClose", '\\': "Backslash", '|': "Pipe", '`': "Backtick", doublequote: "Doublequote", "'": "Singlequote", ";": "Semicolon", ":": "Colon", ",": "Comma", "<": "Lessthan", ".": "Fullstop", ">": "Greaterthan", "/": "Forwardslash", "?": "Questionmark" }, //to obtain id from symbol on event
      keyTheme: "btn-keyboard",
      buttonTheme: "btn-danger"
    };
  },

  methods: {
    enableKeyboard() {
      document.getElementById('txtBox').focus();
      this.showKeyboard = true
    },
    disableKeyboard(id) {
      this.highlightById(id)
      this.showKeyboard = false;
    },
    //when on screen key is pressed , this methods append value to existing string and highlights the pressed key
    press(key) {
      //checking if a number is pressed on virtual keyboard and exists in array , then  highlight the respective numeric on virtual keyboard
      debugger;
      if (this.keys.includes(key)) {
        this.highlightById(key);
        this.value = `${this.value}${key}`;

      }
      else if (this.symbols.includes(key)) {
        console.log("inside symbols", this.$refs.key)
        this.value = `${this.value}${key}`;
        let symbol_index = 0;
        for (let i = 0; i < this.$refs.key.length; i++) {
          if (this.$refs.key[i].defaultValue === key) {
            console.log("true ")
            symbol_index = i
          }
        }
        console.log("symbol index", symbol_index)
        this.$refs.key[symbol_index].style.backgroundColor = "white"
        setTimeout(() => { this.$refs.key[symbol_index].style.backgroundColor = "#1b1b1b" }, 50)


      }
      else {
        document.getElementById('txtBox').focus();
        this.value = `${this.value}${key}`;
        const index = this.findKey(this.$refs.key, key)
        this.$refs.key[index].style.backgroundColor = "white"
        setTimeout(() => { this.$refs.key[index].style.backgroundColor = "#1b1b1b" }, 50)
      }

    },
    //this function triggers when oncreen key "capslock" is clicked , switches lower case to upper case and vice versa
    toggleLetters() {
      this.smallcase = !this.smallcase;
      if (!this.smallcase) {
        const element = document.getElementById('Capslock')
        element.style.backgroundColor = "white";
        element.style.color = "#1b1b1b"
        element.style.transition = "all 0.5s"
      }
      else {

        const element = document.getElementById('Capslock')
        element.style.backgroundColor = "#1b1b1b";
        element.style.color = "white"
        element.style.transition = "all 0.5s"

      }
    },
    highlightById(id) {
      const element = document.getElementById(id)
      element.style.backgroundColor = "#FFFFFF"
      setTimeout(() => { element.style.backgroundColor = "#1b1b1b" }, 50)
    },
    //this method add single whitespace when spacebar is pressed and add 4 whitespaced when tab key is pressed to existing string when oncreen key "spacebar" is pressed
    pressSpacebar(space, id) {
      if (id === "Space") {
        this.highlightById(id)
        document.getElementById('txtBox').focus();
        this.value = this.value + space;

      }
      if (id === "SquareBracketOpen") {
        this.highlightById(id)
        document.getElementById('txtBox').focus();
        this.value = this.value + space
      }
      if (id === "ParanthesisOpen") {
        this.highlightById(id)
        this.value = this.value + space
        document.getElementById('txtBox').focus();

      }
      if (id === "Backslash") {
        this.highlightById(id)
        this.value = this.value + space
        document.getElementById('txtBox').focus();

      }
      if (id === "Pipe") {
        this.highlightById(id)
        this.value = this.value + space
        document.getElementById('txtBox').focus();
      }
      if (id === "Semicolon") {
        this.highlightById(id)
        this.value = this.value + space
        document.getElementById('txtBox').focus();
      }
      if (id === "Colon") {
        this.highlightById(id)
        this.value = this.value + space
      }
      if (id === "Comma") {
        this.highlightById(id)
        this.value = this.value + space
        document.getElementById('txtBox').focus();
      }
      if (id === "Lessthan") {
        this.highlightById(id)
        this.value = this.value + space
        document.getElementById('txtBox').focus();
      }
      if (id === "Fullstop") {
        this.highlightById(id)
        this.value = this.value + space
        document.getElementById('txtBox').focus();
      }
      if (id === "Greaterthan") {
        this.highlightById(id)
        this.value = this.value + space
        document.getElementById('txtBox').focus();
      }
      if (id === "Forwardslash") {
        this.highlightById(id)
        this.value = this.value + space
        document.getElementById('txtBox').focus();
      }
      if (id === "Questionmark") {
        this.highlightById(id)
        this.value = this.value + spac
        document.getElementById('txtBox').focus();

      }
      if (id === "Singlequote") {
        this.highlightById(id)
        document.getElementById('txtBox').focus();
        this.value = this.value + space
      }
      if (id === "Doublequote") {
        this.highlightById(id)
        document.getElementById('txtBox').focus();
        this.value = this.value + space
      }
      if (id === "SquareBracketClose") {
        console.log("inside square bracket close")
        this.highlightById(id)
        document.getElementById('txtBox').focus();

        this.value = this.value + space
      }
      if (id === "ParanthesisClose") {
        this.highlightById(id)
        document.getElementById('txtBox').focus();
        this.value = this.value + space
      }
      if (id === "Tab") {
        this.highlightById(id)
        document.getElementById('txtBox').focus();
        this.value = this.value + space
      }
      if (id === "Backspace") {
        this.highlightById(id)
        document.getElementById('txtBox').focus();
        this.value = this.value.substring(0, this.value.length - 1);
      }
      if (id === "Tilde") {
        this.highlightById(id)
        document.getElementById('txtBox').focus();
        this.value = this.value + space
      }
      if (id === "Enter") {
        this.highlightById(id)
        document.getElementById('txtBox').focus();
        this.value = this.value + '\r\n';
      }
      if (id === "Backtick") {
        document.getElementById('txtBox').focus();
        this.highlightById(id)
        this.value = this.value + '`'
      }
    },
    //this function switches from numbers to symbols and lowercase to uppercase letters when clicked
    toggle() {
      this.smallcase = !this.smallcase;
      this.symbols_enabled = !this.symbols_enabled;
    },
    //this method highlights the respective key on virtual keyboard when the same key on physical keybord is pressed
    keyPressed(event) {
      document.getElementById('txtBox').focus()

      if (event.key === "Backspace") {

        if (this.value) {
          this.value = this.value.slice(0, -1)
        }
        this.keypressed = event.key.toString();


        document.getElementById("Backspace").style.backgroundColor = "#FFFFFF"
        setTimeout(() => { document.getElementById("Backspace").style.backgroundColor = "#1b1b1b" }, 50)

      }
      if (event.code === "Space") {
        if (this.value) {
          this.value = this.value + " ";
        }
        this.keypressed = event.code.toString();
        console.log(document.getElementById("Space"))
        document.getElementById("Space").style.backgroundColor = "#FFFFFF"
        setTimeout(() => { document.getElementById("Space").style.backgroundColor = "#1b1b1b" }, 50)
      }
      if (event.key === "Shift") {
        this.toggle()
        this.keypressed = event.key.toString();
        document.getElementById("Shift").style.backgroundColor = "#FFFFFF"
        setTimeout(() => { document.getElementById("Shift").style.backgroundColor = "#1b1b1b" }, 50)
      }
      if (event.key === "Enter") {
        this.keypressed = event.key.toString();
        document.getElementById("Enter").style.backgroundColor = "#FFFFFF"
        setTimeout(() => { document.getElementById("Enter").style.backgroundColor = "#1b1b1b" }, 50)
      }
      if (event.key === "CapsLock") {
        this.toggleLetters()
        this.keypressed = event.key.toString();
        if (!this.smallcase) {
          const element = document.getElementById('Capslock')
          element.style.backgroundColor = "white";
          element.style.color = "#1b1b1b"
          element.style.transition = "all 0.5s"
        }
        else {

          const element = document.getElementById('Capslock')
          element.style.backgroundColor = "#1b1b1b";
          element.style.color = "white"
          element.style.transition = "all 0.5s"

        }
      }
      else {
        if (this.keys.includes(parseInt(event.key))) {
          const index = this.findKey(this.$refs.key, event.key)
          this.$refs.key[index].style.backgroundColor = "white"
          setTimeout(() => { this.$refs.key[index].style.backgroundColor = "#1b1b1b" }, 50)
        }
        console.log(this.symbol_arr.includes(event.key))
        if (this.symbol_arr.includes(event.key)) {
          this.highlightById(this.symbol_dict[event.key])
        }
        this.keypressed = event.key.toString();
        const index = this.findKey(this.$refs.key, this.keypressed);
        this.$refs.key[index].style.backgroundColor = "#FFFFFF"
        setTimeout(() => { this.$refs.key[index].style.backgroundColor = "#1b1b1b" }, 50)
      }
    },

    //this method finds the index of on screen button in ref array by matching the key code from physical key and inner text of virtual key
    findKey(arr, key) {
      let index;
      for (let i = 0; i < arr.length; i += 1) {
        if (arr[i].value.toString() === key.toString()) {
          index = i;
        }
        else {
          continue;
        }
      }
      return index;
    },
  },

};
</script>

<style scoped>
.virtual-keyboard {
  padding: 1em;

}

#txtbox {
  white-space: pre-line;
  overflow: scroll;

}

#txtBox:focus {
  box-shadow: none;
  outline: none;
  border-color: #ced4da;

}

.container {
  margin: 25px;
  text-align: center;
  background: #000;
}

h1 {
  color: #26abde;
}

.virtual-keyboard .row {
  text-align: center;
  margin: 0 0 15px;
}

.virtual-keyboard .row.spacebar input {
  padding: 10px 150px;
  width: 100%;
}

.virtual-keyboard input[type='button'] {
  padding: 10px 20px;
  margin: 2px;
  border-radius: 5px;
  border: 3px solid #202c2b;
  background: #1b1b1b;
  color: #fff;

}

.virtual-keyboard input[type='button'].shift-activated {
  background: red;
}

.virtual-keyboard input[type='button'].delete,
.virtual-keyboard input[type='button'].shift {
  text-transform: none;
}

.output {
  margin: 15px;
}

.output input {
  color: white;
  background: black;
  border: 0px;
  line-height: 2.2em;
  text-align: center;
  font-size: 2em;
  width: 100%;
}

.output input:focus {
  outline: none;
  border: 0px;
}

.output ::-webkit-input-placeholder {
  color: white;
  text-align: center;
  font-size: 1em;
}

#Space {
  width: 50%;
}
</style>
