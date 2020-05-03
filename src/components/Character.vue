<template>
	<div class="column is-12-mobile is-4-desktop is-4-tablet">
	  <div class="card">
	    <div class="card-header">
	      <img :src="char.image" :alt="char.name">
	    </div>
	    <div class="card-content">
	      <h3 class="title is-size-4">{{ char.name }}</h3>
	      <button class="button is-success is-rounded is-small" @click="moreDetails(char.id)">Ver más</button>
	    </div>
	  </div>
	</div>
</template>

<script type="text/javascript">
	export default {
		props: ['char'],
		methods: {
			moreDetails (id) {
				this.$emit('showModal', id)
			}
		}
	}
</script>