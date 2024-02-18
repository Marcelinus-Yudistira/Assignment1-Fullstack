<script setup>
    import {ref} from 'vue'

    const chars = ref(['+',7,4,1,0])
    const chars2 = ref(['-',8,5,2, '.'])
    const chars3 = ref(['x',9,6,3,'AC'])
    const operator = ref(['+','-','x',':'])
    const number = ref(0)
    const tempNumber = ref([])
    const decimalNum = ref('')
    const calculateView = ref([])

    const filterFunction = (data) => {
        const lastIndex = ref(tempNumber.value.length-1);
        if(data === 'AC' ){
            resetNumeric()
        }else if(data === '.'){
            decimalNum.value = tempNumber.value[lastIndex.value].toString() + '.';
        }else if(operator.value.includes(data)){
            operandFunction(data, lastIndex.value)
        }else{
            if(decimalNum.value != ''){    
                let numDecimal = parseFloat(decimalNum.value + data.toString()); 
                tempNumber.value[lastIndex.value] = numDecimal
                decimalNum.value = ''
            }else{
                numericFunction(data, lastIndex.value)
            }
        }
        calculateView.value = tempNumber.value.join(' ');
    }

    const resetNumeric = () => {
        tempNumber.value.splice(0)
        number.value = 0
    }

    const operandFunction = (value, id) => {
        if(typeof value === typeof tempNumber.value[id]){
            tempNumber.value[id] = value 
        }else{
            tempNumber.value.push(value) 
        }
    }

    const numericFunction = (value, id) => {
        if(typeof value === typeof tempNumber.value[id]){
            if(Number.isInteger(tempNumber.value[id])){
                tempNumber.value[id] = parseInt(tempNumber.value[id].toString() + value.toString()); 
            }else{
                tempNumber.value[id] = parseFloat(tempNumber.value[id].toString() + value.toString());
            }
        }else{
            tempNumber.value.push(value) 
        }
    }

    const calculate = () => {
        evaluateMulDiv(tempNumber.value)
        evaluateAddSub(tempNumber.value)
        number.value = tempNumber.value[0]
    }

    const evaluateMulDiv = (array) => {
        for (let i = 0; i < tempNumber.value.length; i++) {
            if (tempNumber.value[i] === 'x' || tempNumber.value[i] === ':') {
                let operand1 = tempNumber.value[i - 1];
                let operand2 = tempNumber.value[i + 1];
                let result = tempNumber.value[i] === 'x' ? operand1 * operand2 : operand1 / operand2;
                tempNumber.value.splice(i - 1, 3, result);
                i--;
            }
        } 
    }

    const evaluateAddSub = (array) => {
        for (let i = 0; i < tempNumber.value.length; i++) {
            if (tempNumber.value[i] === '+' || tempNumber.value[i] === '-') {
                let operand1 = tempNumber.value[i - 1];
                let operand2 = tempNumber.value[i + 1];
                let result = tempNumber.value[i] === '+' ? operand1 + operand2 : operand1 - operand2;
                tempNumber.value.splice(i - 1, 3, result);
                i--;
            }
        }
    }
</script>

<template>
    <div class="calculator">
        <div class="row">
            <div class="card">
                <h5 class="layer">{{ calculateView }}</h5>
                <h2 class="layer">{{ number }}</h2>
            </div>
            <div class="column">
                <div v-for="char in chars">
                    <button class="button2" :class="{ 'buttonOperand': typeof char === 'string' }" @click="filterFunction(char)">{{char}}</button>
                </div>
            </div>
            <div class="column">
                <div v-for="char in chars2">
                    <button class="button2" :class="{ 'buttonOperand': typeof char === 'string' }" @click="filterFunction(char)">{{char}}</button>
                </div>
            </div>
            <div class="column">
                <div v-for="char in chars3">
                    <button class="button2" :class="{ 'buttonOperand': typeof char === 'string' }" @click="filterFunction(char)">{{char}}</button>
                </div>
            </div>
            <div class="column">
                <button class="button2 buttonOperand" @click="filterFunction(':')">:</button>
                <button class="button2 buttonAssign" @click="calculate()">=</button>
            </div>
        </div>
    </div>
</template>

<style>
    .calculator {
        width: 300px;
        margin: 0 auto;
        padding: 20px;
        border: 2px solid #04AA6D;
        border-radius: 10px;
        background-color: #f2f2f2;
    }
    .row::after {
        content: "";
        clear: both;
        display: table;
    }
    .column {
        float: left;
        width: 25%;
    }
    .button {
        border: none;
        color: white;
        width: 100%;
        transition-duration: 0.4s;
        cursor: pointer;
        text-align: right;
    }

    .button2 {
        border: none;
        color: white;
        width: 100%;
        transition-duration: 0.4s;
        cursor: pointer;
        background-color: white; 
        color: black; 
        border: 2px solid #2e8b57;
    }

    .buttonAssign {
        height: 179px;
        background-color: #c7ea46;
    }

    .buttonOperand {
        background-color: #043927;
        color: white;
    }

    .button1 {
        background-color: white; 
        color: black; 
        border: 2px solid #04AA6D;
    }

    .button1:hover {
        background-color: #04AA6D;
        color: white;
    }

    .layer {
        margin-top: 3px;
        margin-right: 10px;
        margin-bottom: 0px;
        margin-left: 10px;
    }
</style>