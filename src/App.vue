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

// ------------------------------------
// Filter, Sort, and Count
// ------------------------------------

// TODO 4: Create activeFilter and activeSort, the visibleAnimals and
// availableCount computed properties, and setFilter() and setSort().
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

			<!-- TODO 4: The AnimalControls component goes here. -->

			<div id="animal-list" aria-live="polite">
				<!-- TODO 1: The loading, error, and empty messages, and the animals, go here. -->
				 <div v-if="loading">Loading...</div>
				 <div v-else-if="error">{{ error }}</div>
  				 <div v-else-if="animals.length === 0">No results found.</div>
				 <div v-else>
					<div v-for="animal in animals" :key="animal.id">{{ animal.name }}</div>
				</div>
			</div>
		</section>
	</main>
</template>
