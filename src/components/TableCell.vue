<template>
	<div class="table_cell">
		<tr>
			<td class="name">
				{{curr_name}}
				<span v-if="curr_name == 'RUB'">🇷🇺</span>
				<span v-if="curr_name == 'USD'">🇺🇸</span>
				<span v-if="curr_name == 'EUR'">🇪🇺</span>
				<span v-if="curr_name == 'GBP'">🇬🇧</span>
			</td>
			<td class="value">
				{{curr}}
			</td>
			<td v-if="(curr / curr_yesterday.rates[curr_name] * 100 - 100).toFixed(3) == 0" class="change_value">
				0 %
				<!--				<br />-->
				<!--				date: {{curr_yesterday.date}}-->
			</td>
			<td v-else-if="calculateChange > 0" class="change_value positive">
				+ {{(curr / curr_yesterday.rates[curr_name] * 100 - 100).toFixed(3)}} %
				<!--				<br />-->
				<!--				date: {{curr_yesterday.date}}-->
			</td>
			<td v-else-if="calculateChange < 0" class="change_value negative">
				- {{-1 * (curr / curr_yesterday.rates[curr_name] * 100 - 100).toFixed(3)}} %
				<!--				<br />-->
				<!--				date: {{curr_yesterday.date}}-->
			</td>
		</tr>
	</div>
</template>

<script>
	export default {
		props: {
			curr: {
				type: Object,
				required: true
			},
			curr_name: {
				type: Object,
				required: true
			},
			curr_yesterday: {
				type: String,
				required: true
			}
		},
		computed: {
			calculateChange: function () {
				return (this.curr / this.curr_yesterday.rates[this.curr_name] * 100 - 100).toFixed(3);
			}
		}
	}
</script>

<style>
	td {
		width: 150px;
		padding: 7px;
	}

	.name {
		/*background-color: #ddd;*/
		font-weight: bold;
		width: 180px;
	}

	.change_value {
		text-align: right;
		width: 110px;
	}

	.positive {
		color: green;
	}

	.negative {
		color: red;
	}

	.table_cell {
		border-bottom: 1px dotted black;
		transition: background-color 1s;
	}

	.table_cell:hover {
		background-color: #E4E4E4;
	}

</style>