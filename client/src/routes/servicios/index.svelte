<script>
	import ListaServicios from "../../components/Servicios/ListaServicios.svelte";
	import servicios from "../../stores/servicios";
	import categorias from "../../stores/categorias";

	$: serviciosArray = [];

	let inputServicio = "";
	let filterSelected;
	$: {
		if (filterSelected == "categorias") {
			serviciosArray = [];
			serviciosArray = $categorias;
			console.log("categoriasss")
		} else if (filterSelected == "servicios") {
			serviciosArray = [];
			console.log("servicosssss")
			serviciosArray = $servicios;
		} else {
			//RECTIFICAR TODO ESTO
			serviciosArray = [];
			let indexCategorias = $categorias.map((categoria) => {
				return categoria.id;
			});
			let lastIndex = indexCategorias[indexCategorias.length - 1] + 1;
			let newIndexServicio;
			$servicios = $servicios.map((servicio) => {
				return { ...servicio, id: lastIndex++ };
			});
			serviciosArray = $categorias.concat($servicios);
		}
	}
	$: console.log(serviciosArray);

	$: filtered = serviciosArray.filter((servicio) => {
		inputServicio.toLowerCase();
		return (
			servicio.titulo.toLowerCase().includes(inputServicio) ||
			servicio.titulo.toLowerCase() === inputServicio
		);
	});
</script>

<style>
	.servicios__background {
		display: flex;
		justify-content: center;
		background: url("/banner3.jpg") no-repeat;
		height: 550px;
		background-size: cover;
		width: 100%;
		background-position: center;
	}
	.servicios__busqueda {
		display: flex;
		justify-content: center;
		align-items: center;
		width: 100%;
		max-width: 600px;
		background-color: white;
		padding: 5px;
	}
	.servicios__busqueda__input {
		flex: 1 100%;
	}
	.servicios__busqueda__icon {
		padding: 0px 15px;
	}
	.servicios__busqueda__input input {
		width: 100%;
		height: 100%;
		border-radius: 0;
		border: 0;
	}
	.servicios__busqueda__filter {
		display: flex;
		flex-direction: column;
		align-items: center;
		border-left: 1px solid gray;
	}
	.servicios__busqueda__filter select {
		width: min-content;
		height: 100%;
		border: none;
		color: rgb(105, 105, 105);
		font-weight: 600;
		padding: 10px;
		margin-right: 10px;
	}
	/* SECTION SERVICIOS__LIST */
	.k-content {
		margin-top: -120px; /*efecto 'position:relative y top:-100px'*/
		width: 100%;
		max-width: var(--max-width-content);
		margin-left: auto;
		margin-right: auto;
		padding: 6rem 3rem;
		box-sizing: border-box;
		background-color: rgb(255 255 255);
	}
</style>

<svelte:head>
	<title>Servicios</title>
</svelte:head>
<section class="servicios__background">
	<!-- <h1>Que ser servicio necesitas?</h1> -->
	<div class="k-grid k-flex">
		<div class="servicios__busqueda">
			<span class="servicios__busqueda__icon">
				<i class="fas fa-search" />
			</span>
			<div class="servicios__busqueda__input">
				<input
					type="text"
					bind:value={inputServicio}
					on:change={() => {
						serviciosArray = [];
					}}
					placeholder="¿Qué necesitas?"
					autocomplete="off" />
			</div>
			<div class="servicios__busqueda__filter">
				<select bind:value={filterSelected}>
					<option value="todos">Todos</option>
					<option value="categorias">Categoria</option>
					<option value="servicios">Servicios</option>
				</select>
			</div>
		</div>
	</div>
</section>
<div class="k-content">
	<section class="servicios__list">
		<ListaServicios servicios={filtered} />
	</section>
</div>
