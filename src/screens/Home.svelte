<script>
	import {Link } from "svelte-navigator";
	let name = 'home';
</script>

<h1>{name}!</h1>

<main>
	<nav>
		<Link to="/">Home</Link>
		<Link to="/3d">3D</Link>
	</nav>
</main>