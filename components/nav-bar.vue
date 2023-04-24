<template>
  <div>
    <b-navbar toggleable="lg" type="dark" variant="dark">
      <b-navbar-brand href="#"
        >CSARCH2 Simulation Project: Cache Memory (Full Associative, Least
        Recently Used)</b-navbar-brand
      >

      <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

      <b-collapse id="nav-collapse" is-nav>
        <!---<b-navbar-nav>
          <b-nav-item href="#">How to Use</b-nav-item>
          <b-nav-item href="#">GitHub Repository</b-nav-item>
        </b-navbar-nav>  -->

        <!-- Right aligned nav items -->
        <b-navbar-nav class="ml-auto">
          <b-nav-item-dropdown text="Group Members" right>
            <b-dropdown-item href="mailto:criscela_racelis@dlsu.edu.ph"
              >RACELIS, Criscela Ysabelle
            </b-dropdown-item>
            <b-dropdown-item href="mailto:jamuel_kimhoko@dlsu.edu.ph"
              >KIMHOKO, Jamuel</b-dropdown-item
            >
            <b-dropdown-item href="mailto:ralph_furigay@dlsu.edu.ph"
              >FURIGAY, Ralph</b-dropdown-item
            >
            <b-dropdown-item href="mailto:patricia_nicole_martinez@dlsu.edu.ph"
              >MARTINEZ, Patricia</b-dropdown-item
            >
          </b-nav-item-dropdown>
        </b-navbar-nav>
      </b-collapse>
    </b-navbar>
  </div>
</template>

<script>
export default {
  name: 'NavBar',
}
</script>

<style scoped>
.navbar {
  color: #303f9f;
}
</style>
