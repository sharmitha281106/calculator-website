// --- JAVASCRIPT WITH FLOWING DISPLAY LOGIC ---

let expression = '';
const displayElement = document.getElementById('display');

function updateDisplay() {
// 1. Set the text
displayElement.innerText = expression || '0';

// 2. Trigger Animation
// Remove the class to reset the animation state
displayElement.classList.remove('flow-anim');

// Force a reflow (allows animation to restart immediately)
void displayElement.offsetWidth;

// Add the class back to start the flow animation
displayElement.classList.add('flow-anim');
}

function appendNumber(number) {
expression += number;
updateDisplay();
}

function appendFunction(func) {
expression += func;
updateDisplay();
}

function chooseOperation(op) {
expression += op;
updateDisplay();
}

function clearDisplay() {
expression = '';
updateDisplay();
}

function deleteNumber() {
expression = expression.toString().slice(0, -1);
updateDisplay();
}

function compute() {
try {
let evalString = expression
.replace(/×/g, '*')
.replace(/÷/g, '/')
.replace(/\^/g, '**')
.replace(/π/g, 'Math.PI')
.replace(/e/g, 'Math.E');

const result = new Function('return ' + evalString)();

// Precision
let finalResult = parseFloat(result.toFixed(8));

expression = finalResult.toString();
updateDisplay();

} catch (error) {
expression = 'Error';
updateDisplay();
}
}

// Keyboard support
document.addEventListener('keydown', function(event) {
if ((event.key >= 0 && event.key <= 9) || event.key === '.') {
appendNumber(event.key);
}
if (event.key === '=' || event.key === 'Enter') {
event.preventDefault();
compute();
}
if (event.key === 'Backspace') {
deleteNumber();
}
if (event.key === 'Escape') {
clearDisplay();
}
if (['+', '-', '*', '/'].includes(event.key)) {
chooseOperation(event.key);
}
if (event.key === '^') appendFunction('^');
if (event.key === '(') appendFunction('(');
if (event.key === ')') appendFunction(')');
});