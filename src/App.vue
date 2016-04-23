<template>
  <div id="app">
	<sidebar></sidebar>
	<fcanvas></fcanvas>
  	
  </div>
</template>

<script>
import fcanvas from './components/fcanvas.vue';
import sidebar from './components/sidebar.vue';
export default {
  components: {
  	fcanvas, sidebar
  }
}
</script>

<style>
body {
  font-family: Helvetica, sans-serif;
  	margin: 0;
	padding: 0;
}


#app{

	display: flex;
}
</style>
