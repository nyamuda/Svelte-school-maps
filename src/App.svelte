<script>
  import Router from "svelte-spa-router";
  import { routes } from "./routes";
</script>

<main>
  <Router {routes} />
</main>

<style global>
  @tailwind base;
  @tailwind components;
  @tailwind utilities;
</style>
