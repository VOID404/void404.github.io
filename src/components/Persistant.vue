<template>
    <div class="persistant">
	<p>{{ counter  }}</p>
	<button @click="increment">Increment</button>
    </div>
</template>

<style scoped>
 * {
     font-size: 30px;
 }
</style>

<script>
 import { Component, Prop, Vue } from 'vue-property-decorator';

 var db = null
 
 @Component
 export default class Persistant extends Vue {
     sample () {
	 alert("I was clicked!");
     }

     counter = 0;

     created () {
	 this.openDB()
     }

     openDB () {
	 var request = window.indexedDB.open("HelloDB", 3);

	 request.onupgradeneeded = function (event) {

	     console.log("Updating db")

	     var d = event.target.result;

	     // Create another object store called "names" with the autoIncrement flag set as true.    
	     var objStore = d.createObjectStore("counter");

	     objStore.add(0, "counter");
	 };
	 
	 request.onerror = function(event) {
	     alert("Opening DB failed");
	 };

	 var self = this;
	 request.onsuccess = function(event) {
	     db = event.target.result;
	     console.log("Opened database");
	     self.load()
	 };
     }

     load () {
	 var transaction = db.transaction(["counter"], "readonly");

	 transaction.oncomplete = function(event) {
	     console.log("All done!");
	 };

	 transaction.onerror = function(event) {
	     alert("Loading data failed");
	 };

	 var obj = this

	 var objectStore = transaction.objectStore("counter");
	 var getCounter = objectStore.get("counter")
	 getCounter.onsuccess = function(event) {
	     obj.counter = getCounter.result
	 }

	 getCounter.onerror = function(event) {
	     alert("Loading failed!")
	     console.log(event)
	 }
     }

     increment () {
	 this.counter += 1
	 this.save()
     }

     save () {
	 console.log(db)
	 var transaction = db.transaction(["counter"], "readwrite");
	 
	 // Do something when all the data is added to the database.
	 transaction.oncomplete = function(event) {
	     console.log("All done!");
	 };

	 transaction.onerror = function(event) {
	     alert("Saving data failed");
	 };

	 var objectStore = transaction.objectStore("counter");
	 objectStore.put(this.counter, "counter");
     }
 }
</script>
