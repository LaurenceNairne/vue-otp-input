<script setup lang="ts">
	import otp from './components/otp.vue';
	import { ref } from 'vue';

	const otpValue = ref('');
	const otpError = ref(false);
</script>

<template>
	<otp
		:digit-count='4'
		@update:otp='otpValue = $event'
		@error:otp='otpError = $event'
	/>

	<p v-if="otpError" class="error">Please enter values for all fields!</p>

	<p>The current OTP value is: {{ otpValue }}</p>
</template>

<style>
	p {
		font-family: sans-serif
	}

	.error {
		font-weight: 600;
		color: red;
	}
</style>
