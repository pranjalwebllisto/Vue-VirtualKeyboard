<template >
  <div>
    <input type="textarea" class="form-control" v-model="value" id="txtBox" placeholder="Press any button"
      v-on:keydown="keyPressed" />
    <div class="container">


      <div class="virtual-keyboard">
        <div class="rowZero">
          <input type="button" value="`" class="Backtick" id="Backtick">
          <input type="button" v-for="key in keys" :key="key" @click="press(key)" v-if="!symbols_enabled" :value="key"
            :id="key">
          <input type="button" v-for="symbol in symbols" :key="symbol" @click="press(symbol)" v-if="symbols_enabled"
            :value="symbol">
          <input type="button" value="Backspace" class="Backspace" @click="clear()" id="Backspace">
        </div>
        <div class="rowOne">
          <input type="button" id="Tab" value="Tab">
          <input type="button" :value="letter" v-if="smallcase" v-for="letter in letters[0]" :key="letter" ref="key"
            @click="press(letter)" />
          <input type="button" :value="letter" v-if="!smallcase" v-for="letter in letters_capital[0]" :key="letter"
            ref="key" @click="press(letter)" />
        </div>
        <div class=" rowTwo">
          <input type="button" @click="toggleLetters()" id="Capslock" value="Capslock">
          <input type="button" :value="letter" v-if="smallcase" v-for="letter in letters[1]" :key="letter" ref="key"
            @click="press(letter)" />
          <input type="button" :value="letter" v-if="!smallcase" v-for="letter in letters_capital[1]" :key="letter"
            ref="key" @click="press(letter)" />
          <input type="button" @click="newLine()" id="Enter" value="Enter">
        </div>
        <div class="rowThree">
          <input type="button" :value="letter" v-if="smallcase" v-for="letter in letters[2]" :key="letter" ref="key"
            @click="press(letter)" />
          <input type="button" :value="letter" v-if="!smallcase" v-for="letter in letters_capital[2]" :key="letter"
            ref="key" @click="press(letter)" />
          <input type="button" @click="toggle()" id="Shift" value="Shift">
        </div>
        <div class="rowSpace spacebar">
          <input type="button" value="Spacebar" id="Space" @click="pressSpacebar()">
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
      smallcase: true,
      symbols_enabled: false,
      value: "",
      keys: [1, 2, 3, 4, 5, 6, 7, 8, 9, 0],

      letters: [['q', 'w', 'e', 'r', 't', 'y', 'u', 'i', 'o', 'p'], ['a', 's', 'd', 'f', 'g', 'h', 'j', 'k', 'l'], ['z', 'x', 'c', 'v', 'b', 'n', 'm'], ['shift']],
      letters_capital: [['Q', 'W', 'E', 'R', 'T', 'Y', 'U', 'I', 'O', 'P'], ['A', 'S', 'D', 'F', 'G', 'H', 'J', 'K', 'L'], ['Z', 'X', 'C', 'V', 'B', 'N', 'M'], ['shift']],
      symbols: ['!', '@', '#', '$', '%', '^', '&', '*', '(', ')'],
      keyTheme: "btn-keyboard",
      buttonTheme: "btn-danger"
    };
  },
  methods: {
    press(key) {

      //console.log(this.$refs.key)
      //console.log()
      this.value = `${this.value}${key}`;
      //console.log(this.value);
      const index = this.findKey(this.$refs.key, key)
      //console.log("index is", index)
      this.$refs.key[index].style.backgroundColor = "white"
      setTimeout(() => { this.$refs.key[index].style.backgroundColor = "#1b1b1b" }, 50)

      if (this.keys.includes(key)) {
        console.log("inside if ", this.key.includes(key.toString()))
        const element = document.getElementById(`${key}`)
        element.style.backgroundColor = "#FFFFFF"
        setTimeout(() => { element.style.backgroundColor = "#1b1b1b" }, 50)
      }
    },
    newLine() {
      console.log("adding new line")
      this.value = this.value + "&#13;&#10;";
      console.log(this.value)
    },
    toggleLetters() {
      this.smallcase = !this.smallcase;
    },
    pressSpacebar() {
      const space = " "
      this.value = this.value + space;
    },

    toggle() {
      this.smallcase = !this.smallcase;
      this.symbols_enabled = !this.symbols_enabled;
    },
    keyPressed(event) {

      //console.log(this.$refs)
      //console.log("yopu pressed", event.code)
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
      else {
        //console.log("inside else")
        this.keypressed = event.key.toString();
        //console.log("inside", this.keypressed);
        const index = this.findKey(this.$refs.key, this.keypressed);
        //console.log("INDEX", index)
        this.$refs.key[index].style.backgroundColor = "#FFFFFF"
        setTimeout(() => { this.$refs.key[index].style.backgroundColor = "#1b1b1b" }, 50)
      }
    },
    clear(type) {
      if (type === "all") this.value = "";
      else this.value = this.value.substring(0, this.value.length - 1);
    },
    findKey(arr, key) {

      let index;
      for (let i = 0; i < arr.length; i += 1) {
        //console.log("inside find key", arr[i].value.toString());
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
</style>
