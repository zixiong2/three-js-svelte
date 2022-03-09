<script>
	import App3d from './screens/App3d.svelte';
	import Home from './screens/Home.svelte';
	import { Router, Route, Link } from "svelte-navigator";
	
</script>


<Router>
	<main>
		<Route path="/">
			<Home />
    	</Route>

		<Route path="3d">
			<App3d />
    	</Route>
	</main>
</Router>

