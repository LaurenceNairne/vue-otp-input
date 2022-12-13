<script setup lang="ts">
	import otp from './components/otp.vue';
	import { ref } from 'vue';

	const otpValue = ref('');
</script>

<template>
	<otp
		:digit-count='4'
		@update:otp='otpValue = $event'
	/>

	<p>The current OTP value is: {{ otpValue }}</p>
</template>

<style>
	p {
		font-family: sans-serif
	}
</style>
