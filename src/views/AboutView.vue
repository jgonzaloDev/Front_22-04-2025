<template>
	<div class="about container">
		
		<h2 class="mt-5 mb-5 text-center">Mantenimiento de Usuarios</h2>
	

		<form class="row g-3">
			<div class="col-auto">
				<input type="text" 
				readonly 
				class="form-control-plaintext" 
				id="staticLabel" 
				value="Ingrese username: ">
			</div>
			<div class="col-auto">
				<input type="text" 
				class="form-control" 
				v-model="textUsername"
				id="userInput">
			</div>
			<div class="col-auto">
				<button type="button" 
				@click="searchUser"
				class="btn btn-primary mb-3">
				Buscar Usuarios</button>
			</div>
			<div class="col-auto">
				<button type="button" 
				data-bs-toggle="modal" 
				data-bs-target="#exampleModal" 
				data-bs-whatever="@mdo"
				class="btn btn-success mb-3">
				Crear Usuarios</button>
			</div>
		</form>
			
		<table style="margin-top: 24px;" class="table"
		:items="listardatos" :fields="fields">
			<thead class="table-dark">
				<tr>
				<th scope="col">#ID</th>
				<th scope="col">Nombre</th>
				<th scope="col">Nombre de Usuario</th>
				<th scope="col">Correo</th>
				</tr>	
			</thead>
			<tbody>
				<tr v-for="ld in listardatos" v-bind:key="ld.id">
				<th scope="row">{{ ld.id }}</th>
				<td>{{ ld.name }}</td>
				<td>{{ ld.username }}</td>
				<td>{{ ld.email }}</td>
				</tr>
			</tbody>
			</table>

			<div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <h1 class="modal-title fs-5" id="exampleModalLabel">New message</h1>
        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
      </div>
      <div class="modal-body">
        <form>
          <div class="mb-3">
            <label for="recipient-name" class="col-form-label">Recipient:</label>
            <input type="text" class="form-control" id="recipient-name">
          </div>
          <div class="mb-3">
            <label for="message-text" class="col-form-label">Message:</label>
            <textarea class="form-control" id="message-text"></textarea>
          </div>
        </form>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
        <button type="button" class="btn btn-primary">Send message</button>
      </div>
    </div>
  </div>
</div>
	</div>
</template>

<script>
	import { userapi } from '@/api/userapi'
  	import axios from 'axios'
	export default {
		name: 'App',
		data() {
      return {
        fields: ['id', 'name', 'username', 'email'],
        id: "",
        name: "",
        username: "",
        email: "",
        listardatos: [],
		textUsername: ''
      }
    },

	mounted(){
		`${this.textUsername}`.valueOf("");
		this.searchUser();
	},

	methods: {
		async searchUser () {

		let textUsername = `${this.textUsername}`;
		console.log("user: ", textUsername)
		
		// try {
		// 	const userToFind = await fetch(`${userapi}`)
		// 	const user = await userToFind.json()
		// 	this.userData = user
		// 	console.log(user)
		// 	return user
		// } catch (error) {
		// 	alert('Usuario no encontrado')
		// }
		axios.get(`${userapi}`)
		.then(response => {
			this.listardatos = textUsername == '' ? response.data : response.data.filter(item => item.username == textUsername);
			if(this.listardatos.length == 0){
				alert('Username no encontrado');
			}
			console.log("Lista: ", this.listardatos);

		})
		.catch(function (error){
			alert('Username no encontrado')
			console.log(error);

		})
		.finally(function(){

			// Se ejecuto sin problemas

		});
		}
	}
	}
</script>

<style scoped>
	.about {
		min-height: 60vh;
		max-width: 895px;
		margin: auto;
	}
</style>
