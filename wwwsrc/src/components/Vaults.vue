<template>
<div class="vaults">
  <span v-if="$route.name == 'Vaults'">
    <div class="row justify-vaults-center">
      <div class="offset-md-3 col-6 card">
        <div class="vault">
          <h1>Create Vaults</h1>
          <form v-on:submit.prevent="addVault" class="form">
            <input class="input" type="text" name="name" placeholder=" Name" id="name" v-model="vault.name">
            <input class="input" type="text" name="description" placeholder=" Description" id="description" v-model="vault.description">
            <button class="btn btn-primary btn-success" type="submit">Create</button>
          </form>
          </div>
          <div>
            <h2>Vault List</h2>
             <ul>
             <li v-for="vault in vaults" class="col-sm" :key="vault.id">
              <h5 class="vaultgrp">{{vault.name}}  </h5>
              <p class="vaultgrp">{{vault.description}} </p>
              <button class="btn btn-success" :value="vault.id"  @click="viewVault(vault)" type="submit">View</button>
              <button class="btn far fa-trash-alt" :value="vault.id" @click="deleteVault(vault.id)" type="submit"></button>
              <div class="card-columns">
                 <div v-for="keep in vaultKeeps" v-if="activeVault == vault">
                   {{keep.name}}
                   <img class = "card-img-top" width="150px" :src="keep.contentURL || './static/img/placehold.jpg'"> 
                 </div>
               </div>
               </li>
             </ul>
          </div>
        <div>
        </div>
       </div>
    </div>
  </span>
</div>

</template>
<!-- will use this space to create the vault dashboards -->
<script>
import router from "../router";
import login from "./login";

export default {
  name: "Vaults",
  data() {
    return {
      vault: {
        name: "",
        description: "",
        userId: "",
        vaultId: ""
      },
      keep: {},
      activeVault: {}
    };
  },
  mounted() {
    this.$store.dispatch("authenticate");
    if (this.$store.state.user != ""){
    this.$store.dispatch("getVaults")};
  },

  components: {},
  computed: {
    currentUser() {
      return this.$store.state.user;
    },
    vaults() {
      return this.$store.state.vaults;
    },
    vaultKeeps() {
      return this.$store.state.vaultKeep;
    }
  },
  methods: {
    addVault() {
      console.log("test")
      this.$store.dispatch("createVault", this.vault);
     // this.$store.dispatch("clearResults");
    },
    userVaults() {
      this.$store.dispatch("getVaults", this.vaults);
      this.$store.dispatch("clearResults");
    },
    viewVault(vault) {
      this.activeVault = vault;
      this.$store.dispatch("getVaultKeeps", vault.id);
      // this.$store.dispatch('clearResults')
    },
    deleteVault(vid) {
      this.$store.dispatch("delVault", vid);
    },

    addtoVault(vaultid, keepid) {
      // this.$store.dispatch("addVK", piss, keep.id); //made some changes to add id, not sure if valid
    }
  }
};
</script>
<style>
h2 {
  margin-top: 2rem;
}
.vaultgrp {
  text-align: left;
  margin-top: 1rem;
}
</style>

