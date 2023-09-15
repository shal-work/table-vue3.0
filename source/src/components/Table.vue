<template>
	<table class="table">
		<thead class="table__head">
			<tr class="table__row_header">
				<th class="table__hight" v-for="(title, index) in titles" :key="index">{{title}}</th>
			</tr>
		</thead>

		<tbody class="table__body">
			<tr class="table__row"  v-for="item in pData" :key="item.id">
				<td class="table__down">{{item.date}}</td>
				<td class="table__down">{{item.name}}</td>
				<td class="table__down">{{item.points}}</td>
				<td class="table__down">{{item.distance}}</td>
			</tr>
		</tbody>
		
	</table>
	<!-- pData = {{pData}} -->

</template>


<script>
	import {ref, computed} from 'vue';
	
    export default {
        props: {
            pData: {
                type:  Object
            }
        },
		setup() {
			let titles =[ 'Дата', 'Имя', 'Количество', 'Расстояние' ];
		    return {
                titles 
            };
        }
    }
</script>