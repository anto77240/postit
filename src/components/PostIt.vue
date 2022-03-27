<!-- @format -->

<template>
	<body>
		<h1>{{ msg }}</h1>

		<!-- ici démarre le contenu du composant -->
		<div class="pannel">
			<button @click="addSticky">Ajouter un Post-It</button>
			<button @click="hideDelete" v-if="hide">Arrêter la suppression</button>
			<button @click="hideDelete" v-else>Supprimer un Post-It</button>
		</div>

		<ul>
			<li v-for="sticky in stickys" :key="sticky._id">
				<a href="#">
					<!--<h2>{{ sticky.title }}</h2>-->
					<input
						v-model="sticky.title"
						v-on:keyup.enter="save(sticky._id, sticky.title, sticky.content)"
					/>
					<textarea
						v-model="sticky.content"
						v-on:keyup.enter="save(sticky._id, sticky.title, sticky.content)"
					></textarea>
					<div class="stickyButton">
						<button @click="removeSticky(sticky._id)" class="delete" v-if="hide">
							<img src="../assets/trash.png" />
						</button>
						<button class="delete" v-else :disabled="isDisabled"></button>
						<!--<router-link to="/note">-->
						<button class="arrow" @click="goToNote(sticky._id)">
							<img src="../assets/arrow-right.png" />
						</button>
						<!--</router-link>-->
					</div>
				</a>
			</li>
		</ul>
	</body>
</template>

<script>
//let id = 0;
export default {
	name: "PostIt",
	props: {
		msg: String,
	},

	data() {
		return {
			hide: false,
			stickys: [],
		};
	},
	async mounted() {
		//if (localStorage.getItem("stickys")) {
		try {
			const response = await fetch("http://5.135.119.239:3090/notes", {
				method: "GET",
				headers: {
					"Content-Type": "application/json",
				},
			});
			this.stickys = await response.json();
			this.stickys = this.stickys.notes;
		} catch (e) {
			//localStorage.removeItem("stickys");
		}
		//}
		//if (localStorage.id) {
		//id = localStorage.id;
		//}
	},
	methods: {
		async addSticky() {
			const response = await fetch("http://5.135.119.239:3090/notes", {
				method: "POST",
				headers: {
					"Content-Type": "application/json",
				},
				body: JSON.stringify({ title: "Post-It", content: ["Tapez votre texte"] }),
			});
			console.log(response);

			//console.log(this.content);
			//contents.push({ content: this.content });
			//this.stickys.push({ title: "My Post-it", content: this.contents });
			location.reload();
		},
		async save(id, sticky_title, sticky_content) {
			//console.log(this.stickys);
			//const parsed = JSON.stringify(this.stickys);
			const response = await fetch(`http://5.135.119.239:3090/notes/${id}`, {
				method: "PUT",
				headers: {
					"Content-Type": "application/json",
				},
				body: JSON.stringify({ title: sticky_title, content: [sticky_content] }),
			});
			//this.stickys = await response.json();
			console.log(response);
			//console.log(sticky_title, sticky_content);
			//console.log(this.content);
		},
		hideDelete() {
			this.hide = !this.hide;
		},
		async removeSticky(id) {
			//this.stickys = this.stickys.filter((t) => t !== sticky);
			const response = await fetch(`http://5.135.119.239:3090/notes/${id}`, {
				method: "DELETE",
				headers: {
					"Content-Type": "application/json",
				},
			});
			console.log(response);
			location.reload();
		},
		goToNote(id) {
			this.$router.push(`/note/${id}`);
		},
	},
};
</script>

<style scoped>
body {
	background-image: url("../assets/liege.webp");
	color: #fff;
	font-family: "Lato", sans-serif;
	height: 100%;
}

* {
	margin: 0;
	padding: 0;
}

.pannel {
	display: flex;
	justify-content: space-evenly;
}

.stickyButton {
	display: flex;
	justify-content: space-between;
}

button {
	margin-top: 1rem;
	margin-bottom: 1rem;
	padding: 0.8rem;
	background: #10ac84;
	color: white;
	border-radius: 5px;
	font-weight: 700;
	cursor: pointer;
	font-size: 1rem;
}

.delete {
	margin-top: none;
	background: none;
	border: none;
}
.arrow {
	margin-top: none;
	background: none;
	border: none;
	color: black;
	font-weight: 700px;
}

h1 {
	color: black;
	font-size: 3rem;
}

h2 {
	font-weight: bold;
	font-size: 1.5rem;
}
p {
	font-family: "Reenie Beanie", cursive;
	font-size: 1.8rem;
}

ul {
	display: flex;
	flex-wrap: wrap;
	justify-content: center;
}
ul,
li {
	list-style: none;
}

ul li a {
	text-decoration: none;
	color: #000;
	background: #ffc;
	display: block;
	height: 12em;
	width: 12em;
	padding: 0;
	box-shadow: 5px 5px 7px rgba(33, 33, 33, 0.7);
	transform: rotate(-6deg);
	transition: transform 0.15s linear;
}

ul li a:hover,
ul li a:focus {
	box-shadow: 10px 10px 7px rgba(0, 0, 0, 0.7);
	transform: scale(1.25);
	position: relative;
}

ul li {
	margin: 2em;
}

input {
	background: none;
	border: 0px;
	font-size: 1.3rem;
	font-family: "Lato", sans-serif;
	text-align: center;
	width: 190px;

	outline: none;
	resize: none;
}

textarea {
	margin-top: 1rem;
	width: 180px;
	height: 80px;
	background: none;
	border: 0px;
	font-size: 1.8rem;
	font-family: "Reenie Beanie", cursive;
	outline: none;
	resize: none;
}

ul li:nth-child(even) a {
	/* transform: rotate(4deg);*/
	position: relative;
	top: 5px;
	background: #cfc;
}
ul li:nth-child(3n) a {
	/*transform: rotate(-3deg);*/
	position: relative;
	top: -5px;
	background: #ccf;
}
ul li:nth-child(5n) a {
	/*transform: rotate(5deg);*/
	position: relative;
	top: -10px;
}

img {
	width: 20px;
}
</style>
