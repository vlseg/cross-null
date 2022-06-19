<template>
  <div id="app">
	  <div class="name">
	    <h1>Крестики-нолики</h1>
		  <label>Size
			  <input
				  v-model="size"
		      class="size"
		      type="text"
		    >
		  </label>
	    <FieldCrossNull :size="size" />
	  </div>
  </div>
</template>

<script>
import FieldCrossNull from './components/FieldCrossNull.vue'

export default {
  name: 'App',
	data: () => ({
		size: 15
	}),
  components: {
    FieldCrossNull
  },
}
</script>

<style>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: left;
    color: #2c3e50;
  }
  .size {
    width: 20px;
	  margin-bottom: 10px;
	  text-align: center;
  }
	.name {
		padding-left: 28%;
	}
</style>
