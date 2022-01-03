<script lang="ts">
  import Fa from "svelte-fa";
  import { faGithub } from "@fortawesome/free-brands-svg-icons";
  let hamburger_active = false;
  function toggleActive() {
    hamburger_active = !hamburger_active;
  }
  $: hamburger_active_toggle = hamburger_active ? "is-active" : "";
</script>

<nav
  class="navbar is-white has-shadow"
  role="navigation"
  aria-label="main navigation"
>
  <div class="navbar-brand">
    <!-- svelte-ignore a11y-missing-attribute -->
    <a class="navbar-item" href="/">
      <a class="button is-white">
        <img src="assets/logo.png" alt="TLDR-ify Logo" />
      </a>
    </a><a
      role="button"
      class="navbar-burger {hamburger_active_toggle}"
      data-target="navMenu"
      aria-label="menu"
      aria-expanded="false"
      id="navburger"
      on:click={toggleActive}
    >
      <span aria-hidden="true" />
      <span aria-hidden="true" />
      <span aria-hidden="true" />
    </a>
  </div>

  <div class="navbar-menu {hamburger_active_toggle}" id="navMenu">
    <div class="navbar-end">
      <div class="navbar-item">
        <div class="buttons">
          <a
            class="button is-link is-outlined rounded"
            href="https://www.github.com/jacobCOR/tldr-ify-website"
          >
            <span class="icon">
              <Fa icon={faGithub} />
            </span>
            <span>Github</span>
          </a>
        </div>
      </div>
    </div>
  </div>
</nav>

<style>
  .navbar-item {
    text-decoration: none;
  }
</style>
