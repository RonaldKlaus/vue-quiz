<template>
  <div>
    <b-nav tabs>
      <b-nav-item disabled><b>My Quiz App</b></b-nav-item>
      <b-nav-item disabled>
        Counter: {{ this.correctAnswersCount }}/{{ this.totalAnswersCount }}
      </b-nav-item>
    </b-nav>
  </div>
</template>

<script>
  export default {
    props: {
      correctAnswersCount: Number,
      totalAnswersCount: Number,
    }
  }
</script>
