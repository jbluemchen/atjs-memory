<script>
export default{
  name: 'App'
}
</script>

<template>
  <h1>Sound memory</h1>
  <section class="game-board">
    <div class="card"></div>
    <div class="card"></div>
    <div class="card"></div>
    <div class="card"></div>
    <div class="card"></div>
    <div class="card"></div>
    <div class="card"></div>
    <div class="card"></div>
    <div class="card"></div>
    <div class="card"></div>
  </section>
</template>


<style>
#app {
  font-family: 'Courier New', Courier, monospace;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.card {
  border: 5px solid #ccc
}
.game-board{
  display: grid;
  grid-template-columns: 100px 100px 100px 100px;
  grid-template-rows: 100px 100px 100px 100px;
  grid-column-gap: 30px;
  grid-row-gap: 30px;
  justify-content: center;
}
</style>
