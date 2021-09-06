<template>
	<div class="toggle-body">
		<button @click="updateArray()" type="button">schedule processes</button>

		<div>
			<p>Number of Processes:</p>
			<input v-model="processes.length" />
		</div>

		<br>

		<div>
			<h3>Scheduling Algorithm</h3>

			<input @change="updateAlgo()" type="radio" id="fcfs" value="fcfs" v-model="algorithm" />
				<label for="fcfs">First Come First Serve</label>
			<br>
			<input @change="updateAlgo()" type="radio" id="sjf" value="sjf" v-model="algorithm" />
				<label for="sjf">Shortest Job First</label>
			<br>
			<input @change="updateAlgo()" type="radio" id="priority" value="priority" v-model="algorithm" />
				<label for="priority">Priority</label>
			<br>
			<input @change="updateAlgo()" type="radio" id="rr" value="rr" v-model="algorithm" />
				<label for="rr">Round-Robin</label>
			<p @change="updateAlgo()" v-if="algorithm == 'rr'" style="margin-left: 1rem;">
				Time Quantum: <input @change="updateTimeQ()" v-model.number="timeQuantum" />
			</p>
		</div>

		<br>

		<br>

		<table style="">
			<tr>
				<th>Process #</th>
				<th>Arrival Time</th>
				<th>Length</th>
				<th>Priority</th>
			</tr>
			<template v-for="n in processes">
				<tr>
					<td>{{ processes.indexOf(n) }}</td>
					<td><input v-model.number="n.arrivalTime"/></td>
					<td><input v-model.number="n.length" /></td>
					<td><input v-model.number="n.priority" /></td>
				</tr>
			</template>
		</table>
		<br>

		<button type="button">randomize values</button>

	</div>
</template>

<script>
	export default {
		methods: {
			updateArray () {
				this.$emit('updateArray', this.processes);
			},
			updateAlgo () {
				this.$emit('updateAlgo', this.algorithm);
			},
			updateTimeQ () {
				this.$emit('updateTimeQ', this.timeQuantum);
			}
		},
		data () {
			return {
				numberOfProcesses: 1,
				algorithm: 'fcfs',
				timeQuantum: 3,
				processes: [
					{
						arrivalTime: 0.1,
						length: 10,
						priority: 2,
					},
					{
						arrivalTime: 0.4,
						length: 7,
						priority: 1,
					},
					{
						arrivalTime: 0.2,
						length: 2,
						priority: 3,
					},
				],
			}
		}
	}
</script>

<style scoped>
	div {
		font-size: 1rem;
		margin: 1rem;
	}
	.toggle-body {
		width: 20vmin;
	}
	td > input {
		width: 3rem;
	}
</style>
