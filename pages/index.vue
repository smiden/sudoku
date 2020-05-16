<template>
  <div id="content">
    <sudoku />
  </div>
</template>

<script>
import sudoku from '@/components/ds_sudoku'

export default {
  components: {
    sudoku,
  }
}
</script>

<style >
body, div, ul, ol, li, h1, h2, h3, form, p, th, td { 
  padding: 0;margin: 0;
  }
body {
    font-family: Arial, Verdana, sans-serif;
    font-size: 16px;
    color: #434E43;
}
#content {
    clear: both;
    background: #fff;
    padding: 20px;
    position: relative;
    z-index: 1;
    border-radius: 7px;
    border: 1px solid #b8c0b8;
    box-shadow: 0 1px 3px rgba(0, 80, 0, 0.25);
    max-width: 750px;
    margin: 15px auto;
}
</style>

