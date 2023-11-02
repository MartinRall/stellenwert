<script setup lang="ts">
import { onMounted, ref } from 'vue'
import * as romans from 'romans'

const options = [64, 256, 1024, 3999]

const decimal = ref('')
const binary = ref('')
const hexadecimal = ref('')
const roman = ref('')

const range = ref(64);
const solution = ref('')
const direction = ref('d-b');

const test = ref('')
let task: number | string
const correctSolution = ref('')
const color = ref('')


const convertDecimal = () => {
    binary.value = Number(decimal.value).toString(2)
    hexadecimal.value = Number(decimal.value).toString(16)
    roman.value = romans.romanize(Number(decimal.value))
}

const convertBinary = () => {
    decimal.value = parseInt(binary.value, 2).toString()
    hexadecimal.value = parseInt(binary.value, 2).toString(16)
    roman.value = romans.romanize(parseInt(binary.value, 2))
}

const convertHexadecimal = () => {
    decimal.value = parseInt(hexadecimal.value, 16).toString()
    binary.value = parseInt(hexadecimal.value, 16).toString(2)
    roman.value = romans.romanize(parseInt(hexadecimal.value, 16))
}

const convertRoman = () => {
    decimal.value = romans.deromanize(roman.value.toUpperCase()).toString()
    binary.value = Number(romans.deromanize(roman.value.toUpperCase())).toString(2)
    hexadecimal.value = Number(romans.deromanize(roman.value.toUpperCase())).toString(16)
}

const newTest = () => {
    color.value = ''
    solution.value = ''
    task = Math.floor(Math.random() * (range.value - 2)) + 1
    let taskText: any = task
    
    if (direction.value === 'b-d') taskText = Number(task).toString(2)
    if (direction.value === 'r-d') taskText = romans.romanize(Number(task))
    
    test.value = taskText.toString()
}

const checkSolution = () => {
    let valueToCheck: number | string = solution.value
    
    if (direction.value === 'd-r') valueToCheck = Number(romans.deromanize(valueToCheck.toUpperCase())) //.value??????
    else if (direction.value === 'd-b') valueToCheck = Number(parseInt(valueToCheck.toString(), 2))
    else valueToCheck = Number(valueToCheck)

const isCorrect = valueToCheck === Number(task);
color.value = isCorrect ? 'green' : 'red'

correctSolution.value = task.toString();
if (direction.value === 'd-b') correctSolution.value = Number(task).toString(2)
if (direction.value === 'd-r') correctSolution.value = romans.romanize(Number(task))
}
onMounted(newTest)
</script>


<template>
    <h1>Stellenwert</h1>
    <h4>Übungsprogramm für die Klasse 5a RL</h4>
    <div>
        <div class="calculator">
            <h2>Rechner</h2>
            <div class="all-inputs">
                <div class="input-and-label">
                    <label>Dezimal</label>
                    <input type="text" placeholder="DEZ" v-model="decimal" @input="convertDecimal" />
                </div>
                <div class="input-and-label">
                    <label>Binär</label>
                    <input type="text" placeholder="BIN" v-model="binary" @input="convertBinary" />
                </div>
                <div class="input-and-label">
                    <label>Hexadezimal</label>
                    <input type="text" placeholder="HEX" v-model="hexadecimal" @input="convertHexadecimal" @keyup="hexadecimal=hexadecimal.toUpperCase()"/>
                </div>
                <div class="input-and-label">
                    <label>Römisch</label>
                    <input type="text" @keyup="roman=roman.toUpperCase()" placeholder="RÖM < 4000" v-model="roman" @input="convertRoman" />
                </div>
            </div>
        </div>
        <div class="test">
            <h2>Üben</h2>
            <div class="test-container">
                <fieldset>
                <div v-for="v in options">
                    <input :value="v" type="radio" name="range" v-model="range" @change="newTest">
                    <label>bis {{ v }}</label>
                </div>
            </fieldset>
            <fieldset>
                <div>
                    <input value="d-b" type="radio" name="direction" v-model="direction" @change="newTest">
                    <label>Dez in Bin</label>
                </div>
                <div>
                    <input value="b-d" type="radio" name="direction" v-model="direction" @change="newTest">
                    <label>Bin in Dez</label>
                </div>
                <div>
                    <input value="d-r" type="radio" name="direction" v-model="direction" @change="newTest">
                    <label>Dez in Röm</label>
                </div>
                <div>
                    <input value="r-d" type="radio" name="direction" v-model="direction" @change="newTest">
                    <label>Röm in Dez</label>
                </div>
            </fieldset>
            
            <fieldset>
                <p>{{ test }} = <input type="text" v-model="solution" :style="{ color }" @keyup.enter="checkSolution" @keyup="solution=solution.toUpperCase()"/></p>
                
                <p v-if="color === 'red'">Richtige Lösung: {{ correctSolution }}</p>
                <div class="two-buttons">
                    <button @click="newTest">Neue Aufgabe</button>
                    <button v-if="!color" @click="checkSolution">Lösung</button>
                </div>
            </fieldset>
        </div>
    </div>
</div>
</template>

<style scoped></style>
