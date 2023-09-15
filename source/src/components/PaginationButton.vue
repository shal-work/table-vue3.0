<template>
        <div class="btns">
			<template v-if="count == 1">
				<input type="radio" v-bind:id="'radio' + count" name="radios" checked>
				<label v-bind:for="'radio' + count">{{count}}</label>
			</template>
			<template v-else>
				<input type="radio" v-bind:id="'radio' + count" name="radios">
				<label v-bind:for="'radio' + count">{{count}}</label>
			</template>
        </div>
</template>

<script>
    export default {
		props: {
			count:{
				type: Number,
				required: true,
			}
		}
	}
</script>
