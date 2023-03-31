<template>
  <Page>
    <ActionBar class="navbar">
      <Label class="nav-name" text="CALCULATOR" />
    </ActionBar>
    <DockLayout id="calc">
      <StackLayout class="input-label" dock="top" backgroundColor="f0f0f0">
        <GridLayout columns="*" rows="*, *">
          <Label row="0" col="0" class="value" v-model="value" />
          <Label row="1" col="0" class="input" v-model="result" />
        </GridLayout>
      </StackLayout>
      <GridLayout
        columns="*, *, *, *"
        rows="*, *, *, *, *"
        backgroundColor="#ffffff"
        dock="bottom"
      >
        <Button
          @tap="clear()"
          class="button"
          text="C"
          row="0"
          col="0"
          colSpan="2"
          backgroundColor="#dd3d25"
        ></Button>
        <Button
          @tap="enter_exponent()"
          class="button"
          text="^"
          row="0"
          col="2"
          backgroundColor="#25c5dd"
        ></Button>
        <Button @tap="enter_number('7')" class="button" text="7" row="1" col="0"></Button>
        <Button @tap="enter_number('8')" class="button" text="8" row="1" col="1"></Button>
        <Button @tap="enter_number('9')" class="button" text="9" row="1" col="2"></Button>
        <Button @tap="enter_number('4')" class="button" text="4" row="2" col="0"></Button>
        <Button @tap="enter_number('5')" class="button" text="5" row="2" col="1"></Button>
        <Button @tap="enter_number('6')" class="button" text="6" row="2" col="2"></Button>
        <Button @tap="enter_number('1')" class="button" text="1" row="3" col="0"></Button>
        <Button @tap="enter_number('2')" class="button" text="2" row="3" col="1"></Button>
        <Button @tap="enter_number('3')" class="button" text="3" row="3" col="2"></Button>
        <Button
          @tap="enter_number('0')"
          class="button"
          text="0"
          row="4"
          col="0"
          colSpan="2"
        ></Button>
        <Button
          @tap="enter_operand('.')"
          class="button"
          text="."
          row="4"
          col="2"
        ></Button>
        <GridLayout col="3" rowSpan="5" columns="*" rows="*,*,*,*,*">
          <Button
            @tap="enter_operand('+')"
            class="button"
            text="+"
            row="0"
            backgroundColor="#25c5dd"
          ></Button>
          <Button
            @tap="enter_operand('-')"
            class="button"
            text="-"
            row="1"
            backgroundColor="#25c5dd"
          ></Button>
          <Button
            @tap="enter_operand('/')"
            class="button"
            text="/"
            row="2"
            backgroundColor="#25c5dd"
          ></Button>
          <Button
            @tap="enter_operand('*')"
            class="button"
            text="*"
            row="3"
            backgroundColor="#25c5dd"
          ></Button>
          <Button
            @tap="get_answer('=')"
            class="button"
            text="="
            row="4"
            backgroundColor="#25dd99"
          ></Button>
        </GridLayout>
      </GridLayout>
    </DockLayout>
  </Page>
</template>

<script>
export default {
  data() {
    return {
      value: "",
      result: "0",
      hold_operand: false,
    };
  },
  methods: {
    clear() {
      this.value = "";
      this.result = "0";
      this.hold_operand = false;
    },
    get_answer() {
      this.deleteEquals();
      if (!this.lastIsOperand()) {
        this.result = eval(this.value.replace(/\^/g, "**"));
        this.value = this.value.concat("=");
      }
    },
    enter_number(number) {
      this.deleteEquals();
      this.value = this.value.concat(number);
    },
    enter_operand(operand) {
      this.deleteEquals();
      if (this.value.slice(-1) === "") {
        if (operand === "-") {
          if (!this.lastIsOperand()) {
            this.value = this.value.concat(operand);
          }
        }
      } else {
        if (operand === ".") {
          if (!this.hold_operand) {
            if (!this.lastIsOperand()) {
              this.value = this.value.concat(operand);
            }
          }
          this.hold_operand = true;
        } else {
          this.hold_operand = false;
          if (!this.lastIsOperand()) {
            this.value = this.value.concat(operand);
          }
        }
      }
    },
    enter_exponent() {
      this.deleteEquals();
      if (!(this.value.slice(-1) === "")) {
        if (!this.lastIsOperand()) {
          this.value = this.value.concat("^");
        }
      }
    },
    lastIsOperand() {
      this.deleteEquals();
      let operand = this.value.slice(-1);
      if (
        operand === "-" ||
        operand === "+" ||
        operand === "*" ||
        operand === "/" ||
        operand === "." ||
        operand === "^"
      ) {
        return true;
      } else {
        return false;
      }
    },
    deleteEquals() {
      let operand = this.value.slice(-1);

      if (operand === "=") {
        this.value = this.value.slice(0, -1);
      }
    },
    fixPrecision(num1, num2) {
      return Math.abs(num1 - num2) < Number.EPSILON;
    },
  },
};
</script>

<style scoped lang="scss">
@import "@nativescript/theme/scss/variables/blue";

// Custom styles
.fas {
  @include colorize($color: accent);
}

.info {
  font-size: 20;
  horizontal-align: center;
  vertical-align: center;
}
</style>
