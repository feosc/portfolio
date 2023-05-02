<template>
    <div class="form">
        <form action="" v-if="result == false">
            <div class="item">
                <label>Idade</label>
                <input type="text" v-model="form.age" placeholder="Ex: 18">
            </div>
            <div class="item">
                <label>Altura (cm)</label>
                <input type="text" v-model="form.length" placeholder="Ex: 170">
            </div>
            <div class="item">
                <label>Peso (KG)</label>
                <input type="text" v-model="form.weight" placeholder="Ex: 70">
            </div>
            <div class="item">
                <label>Email</label>
                <input type="text" v-model="form.email" placeholder="Ex: 18">
            </div>
            <div class="item">
                <label>Objetivo</label>
                <select type="text" v-model="form.goal">
                    <option v-for="goal in form.goals" :value="goal.value">{{ goal.label }}</option>
                </select>
            </div>
            <div class="item">
                <label>Sexo</label>
                <select type="text" v-model="form.gender">
                    <option v-for="gender in form.genders" :value="gender.value">{{ gender.label }}</option>
                </select>
            </div>
            <div class="item">
                <button @click.prevent="getDiet()">Dieta</button>
            </div>
        </form>
        <ul class="response" v-if="result">
            <li v-for="meal in meals" :key="meal.id">
                <h3>{{ meal.tittle }}</h3>
                <ul>
                    <li v-for="meal in meal.meal" :key="meal">{{ meal }}</li>
                </ul>
            </li>
        </ul>

    </div>
</template>

<script>
import Vue from 'vue'

export default Vue.extend({
    data() {
        return {
            form: {
                age: '',
                length: '',
                gender: '',
                goal: '',
                weight: '',
                email: '',
                goals: [
                    { label: 'Ganho de Massa', value: 'ganho de massa' },
                    { label: 'Manter peso', value: 'manter peso' },
                    { label: 'Perder peso', value: 'perder peso' }
                ],
                genders: [
                    { label: 'Masculino', value: 'masculino' },
                    { label: 'Feminino', value: 'feminino' },
                    { label: 'Outro', value: 'outro sexo' }
                ],
            },
            meals: [],
            result: false
        }
    },
    methods: {
        async getDiet() {

            let text = `Monte uma dieta pra mim para ${this.form.goal}, tenho ${this.form.age} anos, ${this.form.length}cm de altura, peso ${this.form.weight} KG e sou do sexo ${this.form.ge}`

            const apiKey = '';
            const apiUrl = 'https://api.openai.com/v1/engines/text-davinci-003/completions';

            try {
                const response = await fetch(apiUrl, {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json',
                        'Authorization': `Bearer ${apiKey}`
                    },
                    body: JSON.stringify({
                        'prompt': text,
                        'max_tokens': 3000,
                        n: 1,
                        stop: null,
                        temperature: 0.5
                    })
                });

                const data = await response.json();
                const itens = data.choices[0].text.trim().split(/(?=[A-Z])/);

                console.log(itens)
                for (let i = 0; i < itens.length; i++) {
                    const subitensDoItem = itens[i].split("-");
                    let meal = {
                        id: i,
                        tittle: subitensDoItem[0],
                        meal: []
                    }
                    for (let i = 1; i < subitensDoItem.length; i++) {
                        meal.meal.push(subitensDoItem[i])
                    }
                    this.meals.push(meal)
                }

                for (let i = 0; i < this.meals.length; i++) {
                    if (this.meals[i].tittle == '.') {
                        this.meals[i].splice(1, 1)
                    }
                }

                this.result = true

            } catch (error) {
                console.error(error);
            }
        }
    },
})
</script>

<style scoped>
.response {
    display: grid;
    grid-gap: 1rem;

}

form {
    width: 100%;
    border-radius: 6px;
    height: auto;
    background: #FFFFFF;
    box-shadow: 0px 0px 50px 10px rgba(0, 0, 0, 0.1);
    display: grid;
    grid-gap: 1rem;
    padding: 30px;
    padding-top: 20px;
    padding-bottom: 40px;
    height: auto;
}

.item {
    width: 100%;
    display: grid;
    grid-gap: 0.5rem;
}

label {
    color: rgb(0, 0, 0);
    font-size: 14px;
    font-weight: 500;
}

input {
    background-color: rgb(255, 255, 255);
    border: 2px #dbdada solid;
    padding: 10px;
    font-size: 14px;
    font-family: poppins;
    border-radius: 6px;
}

input:focus {
    border: 2px #000 solid;
}

input:hover {
    transition-duration: 0.5s;
    border: 2px #000 solid;
}

input::placeholder {
    font-size: 14px;
    font-family: poppins;
}

select {
    background-color: rgb(255, 255, 255);
    border: 2px #dbdada solid;
    padding: 10px;
    font-size: 14px;
    font-family: poppins;
    border-radius: 6px;
}

button {
    width: 100%;
    padding: 14px;
    border-radius: 6px;
    background-color: #3A7F0D;
    color: #fff;
    font-size: 14px;
    font-family: poppins;
    font-weight: 400;
}

button:hover {
    background-color: #30660c;
    cursor: pointer;
    transition: 0.5s;
}
</style>