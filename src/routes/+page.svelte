<script lang="ts">
	import Team from '../components/Team.svelte';
	let teams: TeamS[] = $state([
		{ name: 'boy', points: 0 },
		{ name: 'girl', points: 0 }
	]);
	// jeopardy-style quiz
	type Question = {
		question: string;
		answer: string;
		points: number;
	};
	type Cagegory = {
		name: string;
		questions: Question[];
	};
	let quiz: Cagegory[] = [
		{
			name: '1',
			questions: [
				{
					question: '1',
					answer: '1',
					points: 100
				},
				{
					question: '2',
					answer: '2',
					points: 200
				},
				{
					question: '3',
					answer: '3',
					points: 300
				},
				{ question: '4', answer: '4', points: 400 },
				{ question: '5', answer: '5', points: 500 }
			]
		},
		{
			name: 'Obscure Details',
			questions: [
				{
					question: 'Gun',
					answer: 'Rize',
					points: 100
				},
				{
					question: "The gender of Chino Kafuu's grandfather in Tippy forms.",
					answer: 'Girl',
					points: 200
				},
				{
					question: 'Chino',
					answer: 'Cute',
					points: 300
				},
				{
					question: 'Cute',
					answer: 'Chino',
					points: 400
				},
				{ question: 'Cute', answer: 'Cute', points: 500 }
			]
		},
		{
			name: 'Obscure Details',
			questions: [
				{
					question: 'Gun',
					answer: 'Rize',
					points: 100
				},
				{
					question: "The gender of Chino Kafuu's grandfather in Tippy forms.",
					answer: 'Girl',
					points: 200
				},
				{
					question: 'Chino',
					answer: 'Cute',
					points: 300
				},
				{
					question: 'Cute',
					answer: 'Chino',
					points: 400
				},
				{ question: 'Cute', answer: 'Cute', points: 500 }
			]
		},
		{
			name: 'Obscure Details',
			questions: [
				{
					question: 'Gun',
					answer: 'Rize',
					points: 100
				},
				{
					question: "The gender of Chino Kafuu's grandfather in Tippy forms.",
					answer: 'Girl',
					points: 200
				},
				{
					question: 'Chino',
					answer: 'Cute',
					points: 300
				},
				{
					question: 'Cute',
					answer: 'Chino',
					points: 400
				},
				{ question: 'Cute', answer: 'Cute', points: 500 }
			]
		},
		{
			name: 'Obscure Details',
			questions: [
				{
					question: 'Gun',
					answer: 'Rize',
					points: 100
				},
				{
					question: "The gender of Chino Kafuu's grandfather in Tippy forms.",
					answer: 'Girl',
					points: 200
				},
				{
					question: 'Chino',
					answer: 'Cute',
					points: 300
				},
				{
					question: 'Cute',
					answer: 'Chino',
					points: 400
				},
				{ question: 'Cute', answer: 'Cute', points: 500 }
			]
		}
	];
	type TeamS = {
		name: string;
		points: number;
	};
	enum status {
		choosequestion,
		question,
		answer,
		score
	}
	class QuizStatus {
		currentQuestion: [number, number] = $state([0, 0]);
		currentTeam = $state(0);
		currentStatus = $state(status.choosequestion);
		donequestions: string[] = $state([]);
	}
	let quizStatus = $state(new QuizStatus());

	function changepoint(team: number, points: number) {
		teams[team].points += points;
	}
</script>

<h1>Welcome to SvelteKit</h1>
<p>Visit <a href="https://kit.svelte.dev">kit.svelte.dev</a> to read the documentation</p>
{#if quizStatus.currentStatus !== status.score}
<span>Boy: <input bind:value={teams[0].points} /></span>
<span>Girl: <input bind:value={teams[1].points} /></span>
{/if}
<div id={status[quizStatus.currentStatus]}>
	{#if quizStatus.currentStatus === status.choosequestion}
		{#each quiz as category}
			<h2>{category.name}</h2>
		{/each}
		{#each quiz as category, i}
			{#each category.questions as question, j}
				<button
					disabled={quizStatus.donequestions.includes(`${i},${j}`)}
					onclick={() => {
						quizStatus.currentQuestion = [i, j];
						quizStatus.currentStatus = status.question;
						console.log(quizStatus);
					}}
				>
					{question.points}
				</button>
			{/each}
		{/each}
	{:else if quizStatus.currentStatus === status.question}
		<div>
			<h2>Question</h2>
			<p>{quiz[quizStatus.currentQuestion[0]].questions[quizStatus.currentQuestion[1]].question}</p>
			<button onclick={() => (quizStatus.currentStatus = status.answer)}>Show Answer</button>
		</div>
	{:else if quizStatus.currentStatus === status.answer}
		<div>
			<h2>Answer</h2>
			<p>{quiz[quizStatus.currentQuestion[0]].questions[quizStatus.currentQuestion[1]].answer}</p>
			<button onclick={() => (quizStatus.currentStatus = status.score)}>Score</button>
		</div>
	{:else if quizStatus.currentStatus === status.score}
		<div>
			<h2>Score</h2>
			<div>
				<Team
					teamname={teams[0].name}
					points={teams[0].points}
					changepoint={() =>
						changepoint(
							0,
							quiz[quizStatus.currentQuestion[0]].questions[quizStatus.currentQuestion[1]].points
						)}
				/>
				<input bind:value={teams[0].points} />
				<Team
					teamname={teams[1].name}
					points={teams[1].points}
					changepoint={() =>
						changepoint(
							1,
							quiz[quizStatus.currentQuestion[0]].questions[quizStatus.currentQuestion[1]].points
						)}
				/>
				<input bind:value={teams[1].points} />
			</div>
			<button
				onclick={() => {
					quizStatus.currentStatus = status.choosequestion;
					quizStatus.donequestions.push(
						`${quizStatus.currentQuestion[0]},${quizStatus.currentQuestion[1]}`
					);
				}}>Next Question</button
			>
		</div>
	{/if}
</div>

<style>
	#choosequestion {
		display: grid;
		grid-template-columns: repeat(5, 1fr);
		grid-template-rows: repeat(5, 1fr);
		grid-column-gap: 0px;
		grid-row-gap: 0px;
	}

	#choosequestion button {
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: 10px;
		border: none;
		background-color: #f0f0f0;
		cursor: pointer;
    height: 70px;
	}

	#choosequestion button:disabled {
		background-color: #ddd;
		cursor: not-allowed;
	}
</style>
