<template>
    <div class="wrapper">
		
	</div>
	<div class="view">
		<div className="conteiner">
			<h1 className="page__title">Таблица рекордов</h1>
			<TableSelect/>
			<!-- <Table :pData = "data" /> -->
		</div>
	</div>
	
	
</template>


<script>
	import TableSelect from '@/components/TableSelect' 
	export default {
		components: { TableSelect }
	}

</script>


