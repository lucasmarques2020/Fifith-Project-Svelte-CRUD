<script>
	let armazenar = [];
	let dados = {
		id: "",
		texto: "",
		estado: false,
	};

	if(localStorage.getItem("armazenar")){
		armazenar = JSON.parse(localStorage.getItem("armazenar"))
	}

	$: localStorage.setItem("armazenar", JSON.stringify(armazenar))
	
	const addDados = () => {
		if (!dados.texto.trim()) {
			alert("Campo vazio! Por favor preencha o campo.");
			dados.texto = "";
			return;
		}
		dados.id = Date.now();
		armazenar = [...armazenar, dados];
		console.log(armazenar);
		dados = {
			id: "",
			texto: "",
			estado: false,
		};
	};
	const deletarDados = id =>{
		armazenar = armazenar.filter(item => item.id !== id)
	}
	const editarDados = id => {
		armazenar = armazenar.map(item => item.id === id ? {...item, estado:!item.estado}: item)
	}
	const classIcono = valor => (
		valor ? 'bi bi-check2' : 'bi bi-pencil-square' 
	)
	const classEstado = valor => valor ? 'btn-success' : 'btn-warning'
</script>

<main>
	<h1>CRUD</h1>
	<h2>Adicione alguma palavra</h2>
	<form on:submit|preventDefault={addDados}>
		<input
			type="text"
			placeholder="Enter"
			class="form-control shadow border-0"
			bind:value={dados.texto}
		/>
	</form>
	{#each armazenar as item}
		<div class="shadow my-3 p-3 lead">
			<p 
			class={item.estado ? 'text-decoration-line-through' : ''} 
			> {item.texto} </p>
			<button class="btn btn-sm {classEstado(item.estado)}" on:click = {editarDados(item.id)}>
				<i class={classIcono(item.estado)} />
			</button>
			<button class="btn btn-sm btn-danger" on:click={deletarDados(item.id)}>
				<i class="bi bi-trash" />
			</button>
		</div>
	{/each}
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>
