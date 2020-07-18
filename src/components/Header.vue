<template>
  <div>
  <b-nav>
    <b-nav-item disabled>
  Correct Answers: {{numCorrect}}/{{numTotal}}
    </b-nav-item>
    </b-nav>
  </div>
</template>

<script>
export default {
    props:[
        'numCorrect',
        'numTotal'
    ]
}
</script>