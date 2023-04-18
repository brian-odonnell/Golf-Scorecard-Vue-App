<template>
    <div class="course-details__back">
        <router-link :to="'/'" exact>Back to Gallery</router-link>
    </div>
    <div v-if="course" class="course-details">
        <h1>{{ course.name }}</h1>
        <!-- <select v-if="yearCount > 1" v-model="selectedYear">
            <option
                v-for="(year, index) in years"
                :key="index"
                :value="year.year"
            >
                {{ year.year }}
            </option>
        </select> -->
        <!-- <p>{{ selectedyear }}</p> -->

        <table class="course-details__scorecard">
            <tr>
                <td class="green">
                    <p>Hole</p>
                </td>
                <!-- <td v-for="hole in courseDetails.holes" class="green">
                    <p>{{ hole.name }}</p>
                </td> -->
                <td class="green">
                    <p>Totals</p>
                </td>
            </tr>
            <tr>
                <td>
                    <p>Yards</p>
                </td>
                <!-- <td v-for="hole in courseDetails.holes">
                    <p>{{ hole.yards }}</p>
                </td> -->
                <!-- <td>
                    <p>{{ totalYards }}</p>
                </td> -->
            </tr>
            <tr>
                <td>
                    <p>Par</p>
                </td>
                <!-- <td v-for="hole in courseDetails.holes">
                    <p>{{ hole.par }}</p>
                </td> -->
                <!-- <td>
                    <p>{{ totalPar }}</p>
                </td> -->
            </tr>
            <tr>
                <td>
                    <p>Score</p>
                </td>
                <!-- <td v-for="(hole, index) in holeScores" :key="hole.id">
                    <input type="number" v-model="holeScores[index].score" @change="updateScores(index)"/>
                </td> -->
                <!-- <td>
                    <p>{{ totalScore }}</p>
                </td> -->
            </tr>
            <tr>
                <td>
                    <p>Difference</p>
                </td>
                <!-- <td v-for="(hole, index) in holeScores" :key="hole.id">
                    <p>{{ formatNumbers(holeScores[index].diff) }}</p>
                </td> -->
                <!-- <td>
                    <p>{{ formatNumbers(totalDiff) }}</p>
                </td> -->
            </tr>
        </table>
    </div>
    <div v-else>
        <p>Loading course details...</p>
    </div>
</template>

<script lang="ts">
	import { ref, onMounted } from 'vue';

	interface HoleInfo {
		number: Number;
		name: String;
		par: Number;
		yards: Number;
	}

	interface Holes {
		year: String;
		holeInfo: HoleInfo[];
	}

	interface Course {
		name: String;
		id: String;
		imagePath: String;
		holes: Holes[];
	}

	export default {
		props: {
			id: {
				type: String,
				required: true
			}
		},

		setup(props) {
			const course = ref<Course[]>([]);
			const isLoading = ref(false);

			onMounted(async () => {
				isLoading.value = true;

				try {
					const response = await fetch ('http://localhost:3000/courses/' + props.id);
					const data = await response.json();
					course.value = data;
				} catch (error) {
					console.error(error);
				}

				isLoading.value = false;
			})

			return {
				course,
				isLoading
			}
		}
	}
</script>