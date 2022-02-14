# 003---Ios-Calculator-JS-03-
## app.js
// Variables
const previousElement = document.querySelector(".previous-display");
const currentElement = document.querySelector(".current-display");

const acButton = document.querySelector(".ac");
const pmButton = document.querySelector(".pm");
const percentButton = document.querySelector(".percent");

const additionButton = document.querySelector(".addition");
const subtractionButton = document.querySelector(".subtraction");
const multiplicationButton = document.querySelector(".multiplication");
const divisionButton = document.querySelector(".division");
const equalsButton = document.querySelector(".equals");

const decimalButton = document.querySelector(".decimal");
const number0 = document.querySelector(".number-0");
const number1 = document.querySelector(".number-1");
const number2 = document.querySelector(".number-2");
const number3 = document.querySelector(".number-3");
const number4 = document.querySelector(".number-4");
const number5 = document.querySelector(".number-5");
const number6 = document.querySelector(".number-6");
const number7 = document.querySelector(".number-7");
const number8 = document.querySelector(".number-8");
const number9 = document.querySelector(".number-9");
const numbersArray = [
  number0,
  number1,
  number2,
  number3,
  number4,
  number5,
  number6,
  number7,
  number8,
  number9,
];

let previousOperand = "";
let currentOperand = "";
let operation = undefined;
let temporaryOperand = "";

// Functions

function DisplayNumbers() {
  if (operation) {
    previousElement.innerHTML = `${previousOperand} ${operation}`;
  } else {
    previousElement.innerHTML = previousOperand;
  }
  currentElement.innerHTML = currentOperand;
}

function AppendNumber(number) {
  if (number === "." && currentOperand.includes(".")) return;
  if (number === 0 && currentOperand === "0") return;
  if (currentOperand.length > 7) return;

  currentOperand = currentOperand.toString() + number.toString();

  DisplayNumbers();
}

function ChooseOperation(selectedOperation) {
  if (temporaryOperand) {
    previousOperand = temporaryOperand.toString();
    currentOperand = "";
    temporaryOperand = "";
    operation = selectedOperation;
    DisplayNumbers();
    return;
  }
  
  operation = selectedOperation;
  previousOperand = currentOperand;
  acButton.innerHTML = "AC";
  currentOperand = "";

  DisplayNumbers();
}

function Compute() {
  let computation;
  const previous = parseFloat(previousOperand);
  const current = parseFloat(currentOperand);

  if (!operation) return;
  if (isNaN(previous) || isNaN(current)) return;

  switch (operation) {
    case "+":
      computation = previous + current;
      break;

    case "-":
      computation = previous - current;
      break;

    case "÷":
      computation = previous / current;
      break;

    case "*":
      computation = previous * current;
      break;

    default:
      break;
  }

  if (isNaN(computation)) return;

  currentOperand = computation;
  previousOperand = "";
  operation = undefined;
  DisplayNumbers();
  temporaryOperand = currentOperand;
  currentOperand = "";
}

function AllClear() {
  if (!previousOperand) {
    currentOperand = currentOperand.slice(0, currentOperand.length - 1);
  } else {
    previousOperand = "";
    currentOperand = "";
    operation = undefined;
    acButton.innerHTML = "C";
  }

  DisplayNumbers();
}

function PlusMinus() {
  currentOperand = currentOperand * -1;
  DisplayNumbers();
}

function Percent() {
  currentOperand = currentOperand / 100;
  DisplayNumbers();
}

// Add event listener to operator buttons

additionButton.addEventListener("click", () => {
  ChooseOperation("+");
});

subtractionButton.addEventListener("click", () => {
  ChooseOperation("-");
});

multiplicationButton.addEventListener("click", () => {
  ChooseOperation("*");
});

divisionButton.addEventListener("click", () => {
  ChooseOperation("÷");
});

equalsButton.addEventListener("click", () => {
  Compute();
});

// Add event listener to top buttons

acButton.addEventListener("click", () => {
  AllClear();
});

pmButton.addEventListener("click", () => {
  PlusMinus();
});

percentButton.addEventListener("click", () => {
  Percent();
});

// Add event listener to number buttons

for (let i = 0; i < numbersArray.length; i++) {
  const number = numbersArray[i];

  number.addEventListener("click", () => {
    AppendNumber(i);
    temporaryOperand = "";
  });
}

decimalButton.addEventListener("click", () => {
  AppendNumber(".");
});

# index.html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta
      name="viewport"
      content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0"
    />
    <title>IOS Calculator</title>
    <link rel="stylesheet" href="style.css" />
  </head>
  <body>
    <div class="calculator">
      <div class="display">
        <div class="previous-display"></div>
        <div class="current-display"></div>
      </div>
      <div class="buttons-container">
        <div class="button function ac">C</div>
        <div class="button function pm">±</div>
        <div class="button function percent">%</div>
        <div class="button operator division">÷</div>
        <div class="button number-7">7</div>
        <div class="button number-8">8</div>
        <div class="button number-9">9</div>
        <div class="button operator multiplication">x</div>
        <div class="button number-4">4</div>
        <div class="button number-5">5</div>
        <div class="button number-6">6</div>
        <div class="button operator subtraction">-</div>
        <div class="button number-1">1</div>
        <div class="button number-2">2</div>
        <div class="button number-3">3</div>
        <div class="button operator addition">+</div>
        <div class="button number-0">0</div>
        <div class="button decimal">,</div>
        <div class="button operator equals">=</div>
      </div>
    </div>

    <script src="app.js"></script>
  </body>
</html>

# style.css

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  user-select: none;
}

body {
  font-family: "Helvetica Neue", sans-serif;
  width: 100vw;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}

.calculator {
  background-color: black;
  border-radius: 50px;
  color: white;
  height: 840px;
  width: 500px;
  padding: 20px;
  position: relative;
}

.display {
  height: 250px;
  width: 100%;
  text-align: right;
  padding: 20px;
  font-size: 100px;
  display: flex;
  flex-direction: column;
  justify-content: space-evenly;
}

.previous-display {
  opacity: 0.5;
  font-size: 50px;
  overflow: hidden;
}

.buttons-container {
  display: grid;
  grid-gap: 10px;
  grid-template-columns: repeat(4, 1fr);
  grid-template-rows: repeat(5, 1fr);
}

.button {
  align-items: center;
  background-color: #333;
  border-radius: 50%;
  cursor: pointer;
  display: flex;
  font-size: 45px;
  height: 100px;
  width: 100px;
  justify-content: center;
  transition: filter .3s;
}

.function {
  color: black;
  background: #a5a5a5;
}

.operator {
  background: #f1a33c;
}

.button.number-0 {
  grid-column: 1 / span 2;
  justify-content: flex-start;
  padding-left: 43px;
  width: 215px;
  border-radius: 55px;
}

.button:active,
.button:focus {
  filter: brightness(120%)
}

.button:hover {
  opacity: 0.5;
}


=========================recap again=======

index.html dosyamızı oluşturduk. head kısmına linl tagı ile style.css dosyamızı bağladık. body kısmının altına script tagı ile app.js mixi bağladık

