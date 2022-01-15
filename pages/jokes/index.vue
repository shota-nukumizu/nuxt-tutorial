<template>
    <div>
       <Jokes v-for="joke in jokes" :key="joke.id" :id="joke.id" :joke="joke.joke" />
    </div>
</template>

<script>
import axios from 'axios'
import Jokes from '../../components/Jokes'

export default {
    components: {
        Jokes
    },
    data() {
        return {
            jokes: []
        }
    },
    async created() {
        const config = {
            headers: {
                'Accept': 'application/json'
            }
        }

        try {
            const res = await axios.get('https://icanhazdadjoke.com/search', config)

            this.jokes = res.data.results
        } catch (error) {
            console.log(error)
        }
    },
    head() {
        return {
            title: 'Welcome to Sample Page',
            meta: [
                {
                    hid: 'description',
                    name: 'description',
                    content: 'Bast Place for sample Page'
                }
            ]
        }
    }
}
</script>

<style>
    
</style>