<!-- @format -->

<template>
	<body>
		<h1>Mon Post-it</h1>
		<!-- ici démarre le contenu du composant -->
		<div class="pannel2">
			<button class="arrow2" @click="goBack()"><img src="../assets/arrow-left.png" /></button>
		</div>
		<ul>
			<li>
				<a href="#">
					<input
						v-model="this.tab.title"
						v-on:keyup.enter="save(this.tab._id, this.tab.title, this.tab.content)"
					/>
					<textarea
						v-model="this.tab.content"
						v-on:keyup.enter="save(this.tab._id, this.tab.title, this.tab.content)"
					></textarea>
					<button @click="removeSticky(this.tab._id)" class="delete">
						<img src="../assets/trash.png" />
					</button>
					<!--<h2>{{ this.tab.title }}</h2>
					<p>{{ this.tab.content }}</p>-->
				</a>
			</li>
		</ul>
	</body>
</template>

<script>
import { useRoute } from "vue-router";

export default {
	name: "OneNote",
	// components: {
	// 	PostIt,
	// },
	props: {
		id: String,
		sticky: [],
	},
	data() {
		return {
			//stickys: [],
			tab: [],
		};
	},
	async mounted() {
		const route = useRoute();
		//var all = JSON.parse(localStorage.getItem("stickys"));
		//this.tab = JSON.parse(localStorage.getItem("stickys"));

		try {
			const id = route.params.id;
			const response = await fetch(`http://5.135.119.239:3090/notes/${id}`, {
				method: "GET",
				headers: {
					"Content-Type": "application/json",
				},
			});
			this.tab = await response.json();
			this.tab = this.tab.note;
		} catch (e) {
			//localStorage.removeItem("stickys");
		}
		//all.forEach(function (item) {
		//if (item._id == route.params._id) {
		//	console.log(item);
		//var title = item.title;
		//this.tab.push(item);
		//console.log(this.tab);
	},
	//return this.tab;
	//});

	methods: {
		goBack() {
			this.$router.push("/");
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
		async removeSticky(id) {
			//this.stickys = this.stickys.filter((t) => t !== sticky);
			const response = await fetch(`http://5.135.119.239:3090/notes/${id}`, {
				method: "DELETE",
				headers: {
					"Content-Type": "application/json",
				},
			});
			console.log(response);
			this.$router.push("/");
		},
	},
};
</script>

<style scoped>
body {
	background-image: url("../assets/liege.webp");
	color: #fff;
	font-family: "Lato", sans-serif;
	height: 100vh;
}

* {
	margin: 0;
	padding: 0;
}

.pannel2 {
	display: flex;
	justify-content: space-evenly;
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

.arrow2 {
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
	font-size: 2.5rem;
	margin-bottom: 40px;
}
p {
	font-family: "Reenie Beanie", cursive;
	font-size: 2.5rem;
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
	height: 25em;
	width: 25em;
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
	z-index: 5;
}

ul li {
	margin: 2em;
}

textarea {
	margin-top: 1rem;
	width: 280px;
	height: 280px;
	background: none;
	border: 0px;
	font-size: 2.8rem;
	font-family: "Reenie Beanie", cursive;
	outline: none;
	resize: none;
}

input {
	background: none;
	border: 0px;
	font-size: 3rem;
	font-family: "Lato", sans-serif;
	text-align: center;
	width: 390px;

	outline: none;
	resize: none;
}

.delete {
	margin-top: none;
	background: none;
	border: none;
}

img {
	width: 50px;
}
</style>
