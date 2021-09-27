<script>
	let armazenar = [];
	let dados = {
		id: "",
		atividade: "",
		data: "",
		estado: false,
	};
	if (localStorage.getItem("armazenar")) {
		armazenar = JSON.parse(localStorage.getItem("armazenar"));
	}

	$: localStorage.setItem("armazenar", JSON.stringify(armazenar));

	const addDados = () => {
		if (!dados.atividade.trim() && !dados.data.trim()) {
			alert("Campo vazio! Por favor preencha o campo.");
			dados.atividade = "";
			dados.data = "";
			return;
		}
		console.log(armazenar);
		dados.id = Date.now();
		armazenar = [...armazenar, dados];
		console.log(armazenar);
		dados = {
			id: "",
			atividade: "",
			data: "",
			estado: false,
		};
	};
	const deletarDados = (id) => {
		armazenar = armazenar.filter((item) => item.id !== id);
	};
	const editarDados = (id) => {
		armazenar = armazenar.map((item) =>
			item.id === id ? { ...item, estado: !item.estado } : item
		);
	};
	const classIcono = (valor) =>
		valor ? "bi bi-check2" : "bi bi-pencil-square";
	const classEstado = (valor) => (valor ? "btn-success" : "btn-warning");
</script>

<main>
	<h1>CRUD ATIVIDADES</h1>
	<div class="container">
		<div class="row">
			<div class="pai-form col-6">
				<form class="form-control shadow border-0">
					<h2>Cadastrar atividade</h2>
					<div class="mb-3">
						<label for="exampleInputdata1" class="form-label"
							>Atividade</label
						>
						<input
							type="text"
							class="form-control"
							placeholder="Atividade"
							bind:value={dados.atividade}
						/>
					</div>
					<div class="mb-3">
						<label for="exampleInputdata1" class="form-label"
							>Tempo</label
						>
						<input
							type="date"
							class="form-control"
							placeholder="data"
							bind:value={dados.data}
						/>
					</div>
					<button
						class="btn btn-primary"
						on:click|preventDefault={addDados}>Cadastrar</button
					>
				</form>
			</div>
			{#each armazenar as item}
				<div class="atividades col-6">
					<h2>Atividades criadas</h2>
					<div class="shadow my-3 p-3 lead">
						<p
							class={item.estado
								? "text-decoration-line-through"
								: ""}
						>
							Usuário: {item.atividade}
						</p>
						<p
							class={item.estado
								? "text-decoration-line-through"
								: ""}
						>
							Data Limite: {item.data}
						</p>
						<button
							class="btn btn-sm {classEstado(item.estado)}"
							on:click={editarDados(item.id)}
						>
							<i class={classIcono(item.estado)} />
						</button>
						<button
							class="btn btn-sm btn-danger"
							on:click={deletarDados(item.id)}
						>
							<i class="bi bi-trash" />
						</button>
					</div>
				</div>
			{/each}
		</div>
	</div>
</main>

<style>
	main {
		height: 100vh;
		width: 100%;
		text-align: center;
		padding: 1em;
	}
	.pai-form {
		display: flex;
		justify-content: center;
	}
	form {
		max-width: 400px;
		max-height: 500px;
		border-radius: 20px;
	}
	h1 {
		text-align: center;
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
