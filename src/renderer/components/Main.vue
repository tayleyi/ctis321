<template>
	<div id="wrapper">
		<main>
			<toggle @updateValues="updateProcesses" @updateAlgo="changeAlgo" @updateTimeQ="changeTimeQ" @updateArray="changeArray"></toggle>
			<AnimationPanel :processList=processes :scheduleAlgo=algorithm :slice=timeQ></AnimationPanel>
		</main>
	</div>
</template>

<script>
	import AnimationPanel from './Main/AnimationPanel';
	import Toggle from './Main/Toggle';

	export default {
		name: 'main',
		components: {
			AnimationPanel, Toggle
		},
		methods: {
			open (link) {
				this.$electron.shell.openExternal(link)
			},
			updateProcesses(event) {
				this.processes = event;
			},
			changeAlgo(event) {
				this.algorithm = event;
			},
			changeTimeQ(event) {
				this.timeQ = event;
			},
			changeArray(event) {
				this.processes = event;
			},
		},
		data () {
			return {
				algorithm: 'fcfs',
				timeQ: 3,
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

<style>
	@import url('https://fonts.googleapis.com/css?family=Public+Sans');

	* {
		box-sizing: border-box;
		margin: 0;
		padding: 0;
	}

	body {
		font-family: 'Public Sans', sans-serif;
		font-size: 2rem;
	}

	#wrapper {
		background:
			radial-gradient(
				ellipse at top left,
				rgba(255, 255, 255, 1) 40%,
				rgba(229, 229, 229, .9) 100%
			);
		height: 100vh;
		padding: 60px 80px;
		width: 100vw;
	}
	main {
	  display: flex;
	  justify-content: space-between;
	}

</style>
