<template>
  <div>
    <b-nav tabs>
      <b-nav-item disabled><b>Quiz App</b></b-nav-item>
      <b-nav-item disabled>Counter: {{index+1}}/10</b-nav-item>
    </b-nav>

    <b-progress class="m-3" :max="10">
      <b-progress-bar :value="index+1" variant="success"></b-progress-bar>      
    </b-progress>
  </div>
</template>

<script>
export default {
  props: {
    index: Object,
  }
}
</script>
