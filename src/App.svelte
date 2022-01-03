<script lang="ts">
  import * as secret from "../secrets.json";
  import Navbar from "./lib/Navbar.svelte";
  import "bulma/css/bulma.css";
  import Main from "./lib/Main.svelte";
  import Footer from "./lib/Footer.svelte";
</script>

<Navbar />
<Main {secret} />
<Footer />
