<template>
    <div class="home">
	<!-- <img alt="Vue logo" src="../assets/logo.png"> -->
	<Persistant />
    </div>
</template>

<script lang="ts">
 import { Component, Vue } from 'vue-property-decorator';
 //import HelloWorld from '@/components/HelloWorld.vue'; // @ is an alias to /src
 import Persistant from '@/components/Persistant.vue'; // @ is an alias to /src

 @Component({
     components: {
	 Persistant,
     },
 })
 export default class Home extends Vue {}
</script>
