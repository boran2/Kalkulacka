<template>
  <v-card
    :loading="loading"
    class="mx-auto my-12 rounded grey white--text darken-4"
  >
    <v-card-title>Calculator 3000</v-card-title>
    <v-card-text>
      <v-input
        class="text-center white rounded px-2 pt-2 subtitle-1"
        height="12px"
      >
        {{ expression }}
      </v-input>
      <v-card-actions class="justify-center">
        <v-btn
          v-if="expressionHistory.length > 0"
          icon
          @click="show = !show"
          justify="center"
        >
          <v-icon class="white--text">{{
            show ? "mdi-chevron-up" : "mdi-chevron-down"
          }}</v-icon>
        </v-btn>
      </v-card-actions>

      <v-expand-transition>
        <div v-show="show">
          <v-card-text>
            <v-input
              v-for="(item, index) in expressionHistory"
              :key="index"
              class="white--text"
            >
              {{ item }}
            </v-input>
          </v-card-text>
        </div>
      </v-expand-transition>
      <v-row no-gutters class="text-center">
        <v-col :cols="3" class="px-2 mb-4">
          <v-btn v-on:click="clear()" block x-large color="amber darken-2"
            >{{row1SpecialChars[0]}}</v-btn
          >
        </v-col>
        <v-col :cols="3" class="px-2 mb-4">
          <v-btn v-on:click="erase()" block x-large color="amber darken-2"
            >{{row1SpecialChars[1]}}</v-btn
          >
        </v-col>
        <v-col
          v-for="(item, index) in row1Chars"
          :cols="3"
          :key="index"
          class="px-2 mb-4"
        >
          <v-btn
            v-on:click="print('/')"
            block
            x-large
            color="blue-grey darken-1"
            >{{ item }}</v-btn
          >
        </v-col>
      </v-row>
      <v-row no-gutters class="text-center">
        <v-col
          v-for="(item, index) in row2Chars"
          :cols="3"
          :key="index"
          class="px-2 mb-4"
        >
          <v-btn
            v-on:click="print(item)"
            block
            x-large
            color="blue-grey darken-1"
            >{{ item }}</v-btn
          >
        </v-col>
      </v-row>
      <v-row no-gutters class="text-center">
        <v-col
          :cols="3"
          v-for="(item, index) in row3Chars"
          :key="index"
          class="px-2 mb-4"
        >
          <v-btn
            v-on:click="print(item)"
            block
            x-large
            color="blue-grey darken-1"
            >{{ item }}</v-btn
          >
        </v-col>
      </v-row>
      <v-row no-gutters class="text-center">
        <v-col
          :cols="3"
          v-for="(item, index) in row4Chars"
          :key="index"
          class="px-2 mb-4"
        >
          <v-btn
            v-on:click="print(item)"
            block
            x-large
            color="blue-grey darken-1"
            >{{ item }}</v-btn
          >
        </v-col>
      </v-row>
      <v-row no-gutters class="text-center">
        <v-col :cols="4" class="px-2 mb-4">
          <v-btn v-on:click="print(row5Chars[0])" block x-large color="blue-grey darken-1">
            {{row5Chars[0]}}
          </v-btn>
        </v-col>
        <v-col :cols="8" class="px-2 mb-4">
          <v-btn v-on:click="equals()" block x-large color="blue-grey darken-1">
            {{row5Chars[1]}}
          </v-btn>
        </v-col>
      </v-row>
    </v-card-text>
  </v-card>
</template>

<script>
export default {
  name: "Calculator",

  data: () => ({
    row1SpecialChars: ["AC", "CE"],
    row1Chars: ["/", "*"],
    row2Chars: [7, 8, 9, "-"],
    row3Chars: [4, 5, 6, "+"],
    row4Chars: [1, 2, 3, "."],
    row5Chars: [0,"="],
    expression: "",
    expressionHistory: [],
    show: false,
  }),
  methods: {
    // prida vyrazu stlacene znaky
    print(option) {
      this.expression = this.expression + option;
    },
    // vymaze vyraz
    clear() {
      this.expression = "";
    },
    // vymaze posledny znak vyrazu
    erase() {
      this.expression = this.expression.slice(0, -1);
    },
    // vypocita vyraz
    equals() {
      var result = 0;
      result = this.calculateByOperation("+", result);
      result = this.calculateByOperation("*", result);
      result = this.calculateByOperation("/", result);
      result = this.calculateByOperation("-", result);
      this.expression = "";
      this.print(result);
    },
    // odchyt  chyby
    notaNumber(result) {
      if (isNaN(result)) {
        return "Error!";
      } else return result;
    },
    // viac ako 2 operatory vo vyraze
    tooManyOperands(array, result) {
      if (array.length > 2) {
        result = "Too many operands for this calculator!";
        this.expressionHistory.push(result);
      }
      return result;
    },
    // vypocet na zaklade operatora
    calculateByOperator(operator, array) {
      switch (operator) {
        case "+":
          return Number(array[0]) + Number(array[1]);
        case "-":
          return Number(array[0]) - Number(array[1]);
        case "/":
          return Number(array[0]) / Number(array[1]);
        case "*":
          return Number(array[0]) * Number(array[1]);
      }
    },
    // vypocet operacie
    calculateByOperation(operator, result) {
      var array = [];
      if (this.expression.includes(operator)) {
        array = this.expression.split(operator);
        result = this.tooManyOperands(array, result);
        if (array.length <= 2) {
          result = this.calculateByOperator(operator, array);
          result = this.notaNumber(result);
          if (Number(array[1]) == 0 && operator == "/") {
            result = "Error div by zero!";
          }
          this.expressionHistory.push(
            array[0] + operator + array[1] + "=" + result
          );
        }
      }
      return result;
    },
  },
};
</script>
