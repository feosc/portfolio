<template>
    <div class="intro">
        <Container class="align">
            <Slogan />
            <h1>{{ this.response }}</h1>
        </Container>
    </div>
</template>

<script>
import Vue from 'vue'

export default Vue.extend({
    mounted() {
        this.getDiet(this.form.age, this.form.length, this.form.gender, this.form.goal)
    },
    data() {
        return {
            form: {
                age: '25',
                length: '170',
                gender: 'masculino',
                goal: 'ganho de massa'
            },
            response: ''
        }
    },
    methods: {
        async getDiet(age, length, gender, goal) {

            let text = `Monte uma dieta pra mim para ${goal}, tenho ${age} anos, ${length}cm de altura e sou do sexo ${gender}`

            const apiKey = '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';
            const apiUrl = 'https://api.openai.com/v1/engines/davinci-codex/completions';

            try {
                const response = await fetch(apiUrl, {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json',
                        'Authorization': `Bearer ${apiKey}`
                    },
                    body: JSON.stringify({
                        'prompt': text,
                        'max_tokens': 100
                    })
                });

                const data = await response.json();
                this.response = data.choices[0].text.trim();
            } catch (error) {
                console.log('Falhou');
                console.error(error);
            }
        }
    },
})
</script>

<style scoped>
.intro {
    height: 100vh;
    width: 100%;
}

.align {
    display: grid;
    grid-template-columns: 1fr, 1fr;
}
</style>