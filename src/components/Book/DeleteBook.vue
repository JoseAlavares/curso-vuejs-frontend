<template>
	<div class="container">
		<div class="row">
			<div class="col">
				<h3>¿Estas seguro que deseas eliminar este libro?</h3>
				<p>Titulo: {{ this.element.title }}</p>
				<p>Descripción: {{ this.element.description }}</p>
			</div>
		</div>

		<div class="row">
			<div class="col">
				<b-button v-on:click="deleteBook" variant="danger">Eliminar</b-button>
			</div>
		</div>
	</div>	
</template>

<script>
	import axios from 'axios'
	import swal from 'sweetalert'

	export default {
		data () {
			return {
				bookId: this.$route.params.bookId,
				element: {
					title: '',
					description: ''
				}
			}
		},

		methods: {
			deleteBook (evt) {
				evt.preventDefault()
				
				const path = `http://localhost:8000/api/v1.0/books/${this.bookId}/`

				axios.delete(path).then(response => {
					swal("Eliminado correctamente", "", "success")
					setTimeout(() => {
						location.href = '/books'
					}, 3000)
					
				})
				.catch(err => {
					console.error(err)
					swal("Ocurrio un errro", "", "error")	
				})
			},

			getBook () {
				const path = `http://localhost:8000/api/v1.0/books/${this.bookId}`

				axios.get(path).then(response => {
					this.element.title = response.data.title
					this.element.description = response.data.description
				})
				.catch(err => {
					console.error(err)
				})
			}

		},		

		created () {
			this.getBook()
		}
	}
</script>

<style lang="css" scoped>
</style>