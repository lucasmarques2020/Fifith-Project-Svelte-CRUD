<script>
	let armazenar = [];
	let dados = {
		id: "",
		usuario: "",
		senha: "",
		email: "",
		estado: false,
	};
	if (localStorage.getItem("armazenar")) {
		armazenar = JSON.parse(localStorage.getItem("armazenar"));
	}

	$: localStorage.setItem("armazenar", JSON.stringify(armazenar));

	const addDados = () => {
		if (
			!dados.usuario.trim() &&
			!dados.senha.trim() &&
			!dados.email.trim()
		) {
			alert("Campo vazio! Por favor preencha o campo.");
			dados.usuario = "";
			dados.senha = "";
			dados.email = "";
			return;
		}
		console.log(armazenar);
		dados.id = Date.now();
		armazenar = [...armazenar, dados];
		console.log(armazenar);
		dados = {
			id: "",
			usuario: "",
			senha: "",
			email: "",
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
	<h1>CRUD</h1>
	<div class = "col-6">
		<form
		class="form-control shadow border-0 position-absolute top-50 end-0 translate-middle"
	>	<h2>Cadastrar</h2>
		<div class="mb-3">
			<label for="exampleInputEmail1" class="form-label">Usuário</label>
			<input
				type="text"
				class="form-control"
				placeholder="Usuário"
				bind:value={dados.usuario}
			/>
		</div>
		<div class="mb-3">
			<label for="exampleInputEmail1" class="form-label">Email</label>
			<input
				type="email"
				class="form-control"
				placeholder="email"
				bind:value={dados.email}
			/>
		</div>
		<div class="mb-3">
			<label for="exampleInputPassword1" class="form-label">Senha</label>
			<input
				type="password"
				class="form-control"
				placeholder="senha"
				bind:value={dados.senha}
			/>
		</div>
		<button class="btn btn-primary" on:click|preventDefault={addDados}
			>Cadastrar</button
		>
	</form>
	</div>
	
	{#each armazenar as item}
	
		<div class= "col-6">
			<h2>Contas criadas</h2>
			<div class="shadow my-3 p-3 lead">
				<p class={item.estado ? "text-decoration-line-through" : ""}>
					Usuário: {item.usuario}
				</p>
				<p class={item.estado ? "text-decoration-line-through" : ""}>
					Email: {item.email}
				</p>
				<p class={item.estado ? "text-decoration-line-through" : ""}>
					Senha: {item.senha}
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
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}
	form {
		width: 400px;
		height: 500px;
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
