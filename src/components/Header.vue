<template>
  <div>
    <b-card title="Card Title" no-body>
      <b-card-header header-tag="nav">
        <b-nav>
          <b-nav-item disabled><b>My Quiz App</b></b-nav-item>
          <b-nav-item disabled>Correct: {{ numCorrect }}/ {{ numTotal }}</b-nav-item>
        </b-nav>
      </b-card-header>
    </b-card>
  </div>
</template>

<script>
export default {
  props : ['numCorrect', 'numTotal']
}
</script>