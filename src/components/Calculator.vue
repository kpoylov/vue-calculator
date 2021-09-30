<template>
    <div class="calculator">
        <div class="top">
            <button @click="deleteValue" class="delete">x</button>
        </div>
        <div class="middle">
            <small v-if="selectedOperation">{{ prevNum }} {{ selectedOperation }} {{ currentNum }}</small>
            <div>
                <h1>{{currentNum}}</h1>
            </div>
        </div>
        <div class="bottom">
            <button @click="pressed('c')">c</button>
            <button @click="pressed('()')">()</button>
            <button @click="pressed('%')">%</button>
            <button @click="pressed('/')">/</button>

            <button @click="pressed('7')">7</button>
            <button @click="pressed('8')">8</button>
            <button @click="pressed('9')">9</button>
            <button @click="pressed('*')">*</button>

            <button @click="pressed('4')">4</button>
            <button @click="pressed('5')">5</button>
            <button @click="pressed('6')">6</button>
            <button @click="pressed('-')">-</button>

            <button @click="pressed('1')">1</button>
            <button @click="pressed('2')">2</button>
            <button @click="pressed('3')">3</button>
            <button @click="pressed('+')">+</button>
            
            <button @click="negateValue">+/-</button>
            <button @click="pressed('0')">0</button>
            <button @click="pressed('.')">.</button>
            <button @click="pressed('=')">=</button>
        </div>
    </div>
</template>
<script>
import { ref, onMounted } from "vue";
export default {
    setup() {
        const operations = ["+", "-", "*", "/", "%", "()"];
        const numbers = ["1", "2", "3", "4", "5", "6", "7", "8", "9", "0", "."];
        const currentNum = ref("");
        const prevNum = ref("");
        const selectedOperation = ref("");
        const total = ref('');

        const pressed = (value) => {
            if (value === "=" || value === "Enter") calculate();
            else if (value === "%") percentage();
            else if (value === "c") clear();
            else if (operations.includes(value)) applyOperation(value);
            else if (numbers.includes(value)) appendNumber(value);
        }
        const applyOperation = (value) => {
            calculate();
            prevNum.value = currentNum.value;
            currentNum.value = "";
            selectedOperation.value = value;
        }
        const appendNumber = (value) => {
            currentNum.value = currentNum.value + value;
        }
        const calculate = () => {
            if (selectedOperation.value === "*") multiply();
            if (selectedOperation.value === "()") multiply();
            else if (selectedOperation.value === "/") divide();
            else if (selectedOperation.value === "%") percentage();
            else if (selectedOperation.value === "-") subtract();
            else if (selectedOperation.value === "+") sum();
            prevNum.value = "";
            selectedOperation.value = "";
        }
        const multiply = () => {
            currentNum.value = prevNum.value * currentNum.value;
            total.value = currentNum.value;
        }
        const divide = () => {
            currentNum.value = prevNum.value / currentNum.value;
            total.value = currentNum.value;
        }
        const percentage = () => {
            currentNum.value = currentNum.value / 100;
            total.value = currentNum.value;
        }
        const subtract = () => {
            currentNum.value = prevNum.value - currentNum.value;
            total.value = currentNum.value;
        }
        const sum = () => {
            currentNum.value = +prevNum.value + +currentNum.value;
            total.value = currentNum.value;
        }
        const handleKeydown = (e) => {
            pressed(e.key);
        }
        const clear = () => {
            currentNum.value = ""
            prevNum.value = ""
            selectedOperation.value = ""
            currentNum.value = ""
        }
        const negateValue = () => {
            if(currentNum.value  === '0') {
                return currentNum.value  = '-0'
            }
            if(currentNum.value === '-') {
            return currentNum.value = '0'
            }
            if(currentNum.value === '0.') {
                return currentNum.value = `-${currentNum.value}`
            }
            if(currentNum.value === '-0.') {
                return currentNum.value = '0.'
            }
            currentNum.value = `${currentNum.value * -1}` 
        }

        const deleteValue = () => {
            if(currentNum.value.length !== 0 && currentNum.value !== '0') {
                currentNum.value = currentNum.value.slice(0, -1)
            }
        }   

        onMounted(() => window.addEventListener('keydown', handleKeydown));
        return { currentNum, pressed, selectedOperation, prevNum, total, negateValue, deleteValue };
    }
}
</script>

<style>
    .calculator {
        background-color: #2C4251;
        width: 20rem;
        border-radius: 2rem;
        margin: auto;
        position: relative;
        min-height: 200px;
    }
    .top {
        display: flex;
        justify-content: flex-end;
        margin-bottom: 2rem;
        padding: 1rem;
    }
    .delete {
        height: 1.6rem;
        width: 1.6rem;
        border-radius: 50px;
        cursor: pointer;
        border: none;
    }
    .delete:hover {
        background-color: #ccc;
    }
    .middle {
        text-align: center;
        margin-bottom: 3rem;
        height: 6rem;
    }
    .middle div{
        overflow-x: hidden;
        margin-top: 1rem;
    }
    .middle small {
        font-weight: bold;
        color: #FFFFFF;
        font-size: 1.5rem;
    }
    .middle h1 {
        color: white;
        font-size: 2.5rem;
        font-weight: 600;
        letter-spacing: -1px;
    }
    .bottom {
        background-color: #fff;
        padding: 1.4rem 1rem;
        border-radius: 2rem;
        font-size: 1.4rem;
        height: 23rem;
        display: grid;
        grid-template-columns: repeat(auto-fit,minmax(4rem,1fr));
        grid-gap: .5rem;
    }
    .bottom button {
        background-color: #E6E7E8;
        padding-top: 19px;
        border-radius: 1.5rem;
        font-size: 1.65rem;
        display: flex;
        justify-content: center;
        align-content: center;
        color: #808285;
        border: none;
    }
    .bottom button:nth-child(1) {
        background-color: #FA386B;
        color: white;
        padding-top: 10px;
        padding-top: 17px;
    }
    .bottom button:nth-child(2) {
        color: #231F20;
    }
    .bottom button:nth-child(3) {
        color: #231F20;
    }
    .bottom button:nth-child(4) {
        color: #231F20;
    }
    .bottom button:nth-child(8) {
        color: #231F20;
    }
    .bottom button:nth-child(12) {
        color: #231F20;
    }
    .bottom button:nth-child(16) {
        color: #231F20;
    }
    .bottom button:nth-child(20) {
        background-color: #30C117;
        color: white;
    }

    button:focus {
        outline: none;
    }
</style>
