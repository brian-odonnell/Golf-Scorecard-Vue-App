<template>
	<h1>Home</h1>
  
	<div v-if="courses.length" class="course__gallery">
	  	<div v-for="course in courses" key="course.id" class="course">
			<router-link :to="'course/' + course.id">
				<h2>{{ course.name }}</h2>
				<div class="course__img">
					<img :src="require('@/assets/' + course.imagePath)" :alt="course.name">
		  		</div>
			</router-link>
	  	</div>
	</div>
	<div v-else>
	  	<p>Loading courses...</p>
	</div>
  </template>

<script lang="ts">
import { EnumNumberMember } from '@babel/types';
import { ref, reactive, onMounted } from 'vue';

interface HoleInfo {
	number: number,
	name: string,
	par: number,
	yards: number
}

interface Holes {
	year: string,
	holeInfo: HoleInfo[]
}

interface Course {
	name: string,
	id: string,
	imagePath: string,
	holes: Holes[],

}

export default {
	setup() {
		const courses = ref<Course[]>([]);
		const isLoading = ref(false);

		onMounted(async () => {
			isLoading.value = true;

			try {
				const response = await fetch ('http://localhost:3000/courses');
				const data = await response.json();
				courses.value = data;
			} catch (error) {
				console.error(error);
			}

			isLoading.value = false;
		})

		return {
			courses,
			isLoading
		}
	}
}
</script>

<styles lang="scss">
	@import "@/styles/app.scss";
</styles>