<template>
  <div>
    <h1>Welcome to My Sample Page.</h1>
  </div>
</template>

<script>
export default {
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
