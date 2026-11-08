<script setup>
// TODO 4: Declare the activeFilter prop and the filter and sort events.
defineProps({
	activeFilter: String,
});
const emits = defineEmits(["filter", "sort"]);
// Write onSortChange(event).
function onSortChange(event) {
	emits("sort", event.target.value);
}


</script>

<template>
	<!-- TODO 4: The filter buttons and sort menu. See README.md for the markup. -->
	<div class="controls">
		<div class="filters" role="group" aria-label="Filter animals">
		<button type="button" :class="{ active: activeFilter === 'all' }" @click="emits('filter', 'all')">All</button>
		<button type="button" :class="{ active: activeFilter === 'available' }" @click="emits('filter', 'available')">Available</button>
		<button type="button" :class="{ active: activeFilter === 'adopted' }" @click="emits('filter', 'adopted')">Adopted</button>
	</div>

	<label for="sort-animals">Sort by:</label>
	<select id="sort-animals" @change="onSortChange">
		<option value="default">Original order</option>
		<option value="name">Name</option>
		<option value="age">Age</option>
	</select>
</div>
</template>
