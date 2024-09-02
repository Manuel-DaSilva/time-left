<script lang="ts">
	import { onMount } from 'svelte';

	let targetDate = new Date('2024-12-15T00:00:00');
	let daysLeft = 0;
	let weekendsLeft = 0;

	let workdaysLeft = 0;
	let weeksLeft = 0;
	let actualTimeLeft = '';

	function calculateTime() {
		const currentDate = new Date();
		const timeDifference = targetDate.getTime() - currentDate.getTime();

		daysLeft = Math.floor(timeDifference / (1000 * 60 * 60 * 24));

		const remainingHours = Math.floor((timeDifference % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
		const remainingMinutes = Math.floor((timeDifference % (1000 * 60 * 60)) / (1000 * 60));
		const remainingSeconds = Math.floor((timeDifference % (1000 * 60)) / 1000);

		weekendsLeft = countWeekends(currentDate, targetDate);
		workdaysLeft = countWorkdays(currentDate, targetDate);
		weeksLeft = Math.floor(daysLeft / 7);

		actualTimeLeft = `${daysLeft} days, ${remainingHours} hours, ${remainingMinutes} minutes, ${remainingSeconds} seconds`;
	}

	function countWeekends(startDate: Date, endDate: Date) {
		let count = 0;
		let currentDate = new Date(startDate);

		while (currentDate <= endDate) {
			if (currentDate.getDay() === 0 || currentDate.getDay() === 6) {
				count++;
			}
			currentDate.setDate(currentDate.getDate() + 1);
		}

		return count;
	}

	function countWorkdays(startDate: Date, endDate: Date) {
		let count = 0;
		let currentDate = new Date(startDate);

		while (currentDate <= endDate) {
			if (currentDate.getDay() !== 0 && currentDate.getDay() !== 6) {
				count++;
			}
			currentDate.setDate(currentDate.getDate() + 1);
		}

		return count;
	}

	function startTimer() {
		calculateTime();
		setInterval(calculateTime, 1000);
	}

	onMount(() => {
		startTimer();
	});
</script>

<div class="flex h-screen w-full items-center justify-center">
	<div
		class="mx-auto max-w-md rounded-lg border bg-background p-6 text-card-foreground shadow-md"
		data-v0-t="card"
	>
		<div class="flex flex-col items-center gap-4">
			<h2 class="text-2xl font-bold">Hasta el 15 de diciembre de 2024</h2>
			<div class="grid w-full grid-cols-2 gap-4">
				<div class="flex flex-col items-center gap-1 rounded-md bg-muted p-4">
					<span class="text-4xl font-bold">{daysLeft}</span>
					<span class="text-sm text-muted-foreground">Días</span>
				</div>
				<div class="flex flex-col items-center gap-1 rounded-md bg-muted p-4">
					<span class="text-4xl font-bold">{weekendsLeft}</span>
					<span class="text-sm text-muted-foreground">Fines de semana</span>
				</div>
				<div class="flex flex-col items-center gap-1 rounded-md bg-muted p-4">
					<span class="text-4xl font-bold">{workdaysLeft}</span>
					<span class="text-sm text-muted-foreground">Días laborales</span>
				</div>
				<div class="flex flex-col items-center gap-1 rounded-md bg-muted p-4">
					<span class="text-4xl font-bold">{weeksLeft}</span>
					<span class="text-sm text-muted-foreground">Semanas</span>
				</div>
			</div>
			<p class="text-xs">{actualTimeLeft}</p>
		</div>
	</div>
</div>
