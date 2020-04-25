<template>
	<div class="exchange_rate">
		<div class="header">
			<h1>Foreign exchange rates against the {{currency.base}} 💶 on {{currency.date}}</h1>
			<hr/>
		</div>

		<table class="main_table">
			<div class="table_header_wrapper">
				<tr>
					<th style="width: 180px;">CURRENCY 💰</th>
					<th>RATE ⚖️</th>
					<th style="width: 115px;text-align: right">CHANGE 📈️</th>
				</tr>
			</div>
			<TableCell
					v-for="(curr, curr_name) in currency.rates"
					v-bind:curr="curr"
					v-bind:curr_name="curr_name"
					v-bind:curr_yesterday="currency_yesterday"
			/>
		</table>

		<div class="footer">
			<hr/>
			<h3>CHANGE 📈️ rate is calculated from previous day ( {{currency_yesterday.date}} )</h3>
			<p>made by Zarubin Yurii (@pfh_hbq)</p>
		</div>
	</div>
</template>


<script>
	import TableCell from '@/components/TableCell.vue'

	export default {
		data() {
			return {
				currency: [],
				currency_yesterday: []
			}
		},
		methods: {
			getYesterday: function (jsonNew) {
				this.currency = jsonNew;

				let oldDate = this.currency.date;

				let newDate = oldDate.split('-');

				if (newDate[2] > 1) {
					newDate[2] = newDate[2] - 1;
				} else if (newDate[1] == 1 || newDate[1] == 2 || newDate[1] == 4 || newDate[1] == 6 || newDate[1] == 8
					|| newDate[1] == 9 || newDate[1] == 11) {
					if (newDate[1] == 1)
						newDate[1] = 12;
					else
						newDate[1] = newDate[1] - 1;

					newDate[2] = 31;
				} else if (newDate[1] == 5 || newDate[1] == 7 || newDate[1] == 10 || newDate[1] == 12) {
					newDate[1] = newDate[1] - 1;
					newDate[2] = 30;
				} else if (newDate[1] == 3) {
					if ((newDate[0] % 4 == 0 && newDate[0] % 100 != 0) || newDate[0] % 400 == 0) {
						newDate[2] = 29;
					} else {
						newDate[2] = 28;
					}

					newDate[1] = newDate[1] - 1;
				}

				let yesterday = newDate[0] + '-' + newDate[1] + '-' + newDate[2];

				return yesterday;
			}
		},
		mounted() {
			fetch('https://api.exchangeratesapi.io/latest' + '?base=EUR')
				.then(response => response.json())
				.then(json => fetch('https://api.exchangeratesapi.io/' + this.getYesterday(json) + '?base=EUR')
					.then(response => response.json())
					.then(json => {
						this.currency_yesterday = json
					}))
		},
		components: {
			TableCell
		}
	}

</script>

<style>
	* {
		margin: 0;
		padding: 0;
		font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
	}

	h1 {
		text-align: center;
		padding: 25px 0;
	}

	h3 {
		text-align: center;
		padding: 25px 0;
	}

	hr {
		width: 75%;
		height: 2px;
		margin: 0 auto;
		border-width: 0;
		color: black;
		background-color: black;
	}

	.main_table {
		margin: 25px auto;
		color: black;
		border-collapse: collapse;
		border-spacing: 0;

	}

	th {
		width: 150px;
		padding: 7px;
		text-align: left;
		font-weight: bold;
	}

	.table_header_wrapper {
		border-bottom: 1px dotted black;
	}

	p {
		text-align: center;
		margin-bottom: 25px;
	}


</style>