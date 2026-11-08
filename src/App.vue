<script setup>
/*
 * Animal Adoption Board
 *
 * EXTRA CREDIT: If you recorded the video walkthrough (see README.md), paste
 * its link here. A video that is not linked here will not be graded.
 *
 *   Video link:
 */

import { ref, computed } from "vue";
import AnimalCard from "./components/AnimalCard.vue";
import AnimalControls from "./components/AnimalControls.vue";
// ------------------------------------
// App Data
// ------------------------------------

// TODO 1: Create the app's data with ref(): the animals array, loading,
// and error.

const animals = ref([]);
const loading = ref(false);
const error = ref("");

	
// ------------------------------------
// Load the Animals
// ------------------------------------

async function loadAnimals() {
  loading.value = true;
  error.value = "";
try {
    await new Promise((resolve) => setTimeout(resolve, 2000));
    const response = await fetch("animals.json");
    if (!response.ok) {
      throw new Error("Request failed: " + response.status);
    }
    animals.value = await response.json();
  } catch (err) {
    error.value = "Couldn't load animals. Please try again.";
    console.log(err);
  }
  loading.value = false;
}

loadAnimals();

// ------------------------------------
// Adopt or Return an Animal
// ------------------------------------

// TODO 3: Write toggleAdoption(animal).

function toggleAdoption(animal) {
  animal.adopted = !animal.adopted;
}

// ------------------------------------
// Filter, Sort, and Count
// ------------------------------------

// TODO 4: Create activeFilter and activeSort, the visibleAnimals and
// availableCount computed properties, and setFilter() and setSort().
const activeFilter = ref("all");
const activeSort = ref("default");

const visibleAnimals = computed(() => {
	let visible = animals.value.slice();

	if (activeFilter.value === "available") {
		visible = visible.filter((a) => !a.adopted);
	} else if (activeFilter.value === "adopted") {
		visible = visible.filter((a) => a.adopted);
	}

	if (activeSort.value === "name") {
		visible.sort((a, b) => a.name.localeCompare(b.name));
	} else if (activeSort.value === "age") {
		// A negative result puts a before b, so smaller ages come first.
		visible.sort((a, b) => a.age - b.age);
	}
	return visible;
})

const availableCount = computed(() => {
	let available = animals.value.filter((a) => !a.adopted);
	return available.length;
})

function setFilter(filter) {
	activeFilter.value = filter;
}

function setSort(sort) {
	activeSort.value = sort;
}
</script>

<template>
	<header>
		<h1>Animal Adoption Board</h1>
		<p>Meet the animals currently staying at our shelter.</p>
	</header>

	<main>
		<section aria-labelledby="animals-heading">
			<h2 id="animals-heading">Animals</h2>

			<!-- TODO 4: The available count goes here. -->
			 <div> <p v-if="!loading && !error" id="animal-count">
				{{ availableCount }} of {{ animals.length }} animals are available for adoption.
			</p> </div>

			<!-- TODO 4: The AnimalControls component goes here. -->
			<AnimalControls
				:active-filter="activeFilter"
				@filter="setFilter"
				@sort="setSort"
			/>

			<div id="animal-list" aria-live="polite">
				<!-- TODO 1: The loading, error, and empty messages, and the animals, go here. -->
				 <div v-if="loading">Loading...</div>
				 <div v-else-if="error">{{ error }}</div>
  				 <div class ="empty-message" v-else-if="animals.length === 0">There are no animals to show.</div>
				 <div v-else>
					<AnimalCard v-for="animal in visibleAnimals" :key="animal.id" :animal="animal" @toggle="toggleAdoption(animal)"/>
				</div>
			</div>
		</section>
	</main>
</template>
