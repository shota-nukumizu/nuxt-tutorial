<template>
    <div>
       <h1>About Page</h1>
       <p>This is a sample page made in Nuxt.</p>
    </div>
</template>

<script>
export default {
    head() {
        return {
            title: 'About App',
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