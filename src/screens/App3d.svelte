
<script>
	import { Link } from "svelte-navigator";
	import Threejs from './../components/threejs.svelte';

	let name = '3d';

	
</script>

<h1>{name}!</h1>

<nav>
	<Link to="/">Home</Link>
</nav>

<main>
	<Threejs />
</main>