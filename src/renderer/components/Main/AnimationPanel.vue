<template>
	<div>
		<template v-if="scheduleAlgo == 'fcfs'">
			<h1>First Come, First Serve</h1>
			<span class="container">
				<span class="child-box" :style="{flex: i.length  + ' 0 0' }" v-for="i in sortByTime()">
					{{ i.index }} (t = {{ i.length }})
				</span>
			</span>
		</template>
		<template v-else-if="scheduleAlgo == 'sjf'">
			<h1>Shortest Job First</h1>
			<span class="container">
				<span class="child-box" :style="{flex: i.length  + ' 0 0' }" v-for="i in sortByLength()">
					{{ i.index }} (t = {{ i.length }})
				</span>
			</span>
		</template>
		<template v-else-if="scheduleAlgo == 'priority'">
			<h1>Priority</h1>
			<span class="container">
				<span class="child-box" :style="{flex: i.length  + ' 0 0' }" v-for="i in sortByPriority()">
					{{ i.index }} (t = {{ i.length }})
				</span>
			</span>
		</template>
		<template v-else>
			<h1>Round-Robin with a Time-Quantum of {{ slice }}</h1>
			<span class="container">
				<span class="child-box" :style="{flex: i.length  + ' 0 0' }" v-for="i in sortRoundRobin()">
						{{ i.index }} (t = {{ i.length }})
				</span>
			</span>
		</template>
	</div>
</template>

<script>
	function calculateTotalLength(processArray) {
		let sum = 0;
		for (let i = 0; i < processArray.length; i++) {
			sum += processArray[i].length;
		}
		return sum;
	}
	export default {
		props: [
			'processList',
			'scheduleAlgo',
			'slice'
		],
		methods: {
			sortByTime () {
				let returnedArray = [this.processList[0]];
				returnedArray[0].index = 0;
				for (let i = 1; i < this.processList.length; i++) {
					for (let j = 0; j < returnedArray.length; j++) {
						if (this.processList[i].arrivalTime <= returnedArray[j].arrivalTime) {
							returnedArray.splice(j, 0, this.processList[i]);
							returnedArray[j].index = i;
							break;
						}
						else if (j == returnedArray.length - 1) {
							returnedArray.splice(j + 1, 0, this.processList[i]);
							returnedArray[j + 1].index = i;
							break;
						}
					}
				}
				return returnedArray;
			},
			sortByLength () {
				let returnedArray = [this.processList[0]];
				returnedArray[0].index = 0;
				for (let i = 1; i < this.processList.length; i++) {
					for (let j = 0; j < returnedArray.length; j++) {
						if (this.processList[i].length <= returnedArray[j].length) {
							returnedArray.splice(j, 0, this.processList[i]);
							returnedArray[j].index = i;
							break;
						}
						else if (j == returnedArray.length - 1) {
							returnedArray.splice(j + 1, 0, this.processList[i]);
							returnedArray[j + 1].index = i;
							break;
						}
					}
				}
				return returnedArray;
			},
			sortByPriority () {
				let returnedArray = [this.processList[0]];
				returnedArray[0].index = 0;
				for (let i = 1; i < this.processList.length; i++) {
					for (let j = 0; j < returnedArray.length; j++) {
						if (this.processList[i].priority <= returnedArray[j].priority) {
							returnedArray.splice(j, 0, this.processList[i]);
							returnedArray[j].index = i;
							break;
						}
						else if (j == returnedArray.length - 1) {
							returnedArray.splice(j + 1, 0, this.processList[i]);
							returnedArray[j + 1].index = i;
							break;
						}
					}
				}
				return returnedArray;
			},
			sortRoundRobin () {
console.log('start');
				let originalArray = JSON.parse(JSON.stringify(this.processList));
				let returnedArray = [];
				let counter = 0;
				let counterA = 0;
				while (counter < originalArray.length) {
					for (let i = 0; i < originalArray.length; i++) {
						if (originalArray[i].length > this.slice) {
							let temp = JSON.parse(JSON.stringify(originalArray[i]));
							temp.length = this.slice;
							temp.index = i;
							returnedArray.splice(counterA, 0, temp);

							originalArray[i].length -= this.slice;
							counterA++;
						}
						else if (originalArray[i].length <= 0) {
							continue;
						}
						else {
							returnedArray.splice(counterA, 0, originalArray[i]);
							returnedArray[counterA].index = i;

							originalArray[i].length -= this.slice;
							counterA++;
							counter++;
						}
console.log(counterA + '\t' + returnedArray.length + '\t' + returnedArray[counterA - 1].length);
					}
				}

console.log("returned ");
console.log(returnedArray);
				return returnedArray;
			}
		},
		data () {
			return {
			}
		}
	}
</script>

<style scoped>
	.container {
		display: flex;
		flex-flow: row wrap;
		margin: 2rem 0 2rem 0;
	}
	.child-box {
		border: solid 1px black;
		padding: 1rem;
		margin: 0.2rem;
	}
</style>
