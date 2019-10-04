<template>
    <div>
        <b-nav tabs>
            <b-nav-item active><b>Quiz App</b></b-nav-item>
            <b-nav-item disabled><b>Counter : {{index}}/10</b></b-nav-item>
        </b-nav>
  </div>
</template>

<script>
    export default {
        props: {
            index: String,
            numCorrect: Number,
            numTotal: Number
        }
    }
</script>