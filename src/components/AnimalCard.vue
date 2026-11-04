<script setup>
import dayjs from "dayjs";
// TODO 2: Declare the animal prop with defineProps().
defineProps({
	animal: Object,
});
// TODO 3: Declare the toggle event with defineEmits().
defineEmits(["toggle"]);

function arrivalText(date) {
	return dayjs(date).format("MMM D, YYYY") + " (" + dayjs().diff(date, "day") + " days in shelter)";
}
</script>

<template>
	<!-- TODO 2: Build one animal's card. See README.md for the markup. -->
	<article class="animal-card">
		<div :class="animal.adopted ? 'adopted' : 'available'"></div>
		<h2>{{ animal.name }}</h2>
    	<p>{{ animal.species }}, {{ animal.age }} years old</p>
		<p> Arrived {{ arrivalText(animal.intakeDate) }}</p>
   	 	<p>{{ animal.adopted ? "Adopted" : "Available" }}</p>
		<div :class="animal.adopted ? 'adopted' : 'available'">
   	 	<button @click="$emit('toggle')">Toggle</button>
    	<button @click="$emit('delete')">Delete</button>
		</div>
	</article>
</template>
