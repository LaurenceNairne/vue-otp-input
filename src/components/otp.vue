<script lang='ts' setup>
	import { ref, reactive } from 'vue';

	const props = defineProps({
		default: String,
		digitCount: {
			type: Number,
			required: true,
		}
	});

	const digits = reactive<String[]>([]);

	if (props.default && props.default.length === props.digitCount) {
		for (let i = 0; i < props.digitCount; i++) {
			digits[i] = props.default.charAt(i);
		}
	} else {
		for (let i = 0; i < props.digitCount; i++) {
			digits[i] = '';
		}
	}

	const otpCont = ref<null | { children: HTMLElement[]; }>(null);

	const emit = defineEmits(['update:otp']);

	const handleKeyDown = (event: KeyboardEvent, index: number) => {
		if (event.key !== 'Tab' && event.key !== 'ArrowRight' && event.key !== 'ArrowLeft') {
			event.preventDefault();
		}

		if (event.key === 'Backspace') {
			digits[index] = '';

			if (index !== 0 && otpCont.value) {
				(otpCont.value.children)[index - 1].focus();
			}
		}

		if((new RegExp('^([0-9])$')).test(event.key)) {
			digits[index] = event.key;

			if (index !== props.digitCount - 1 && otpCont.value) {
				(otpCont.value.children)[index + 1].focus();
			}
		}

		emit('update:otp', digits.join(''));
	};
</script>

<template>
	<div ref='otpCont'>
		<input
			type='text'
			class="digit-box"
			maxlength='1'
			v-for='(el, ind) in digits'
			v-model='digits[ind]'
			@keydown='handleKeyDown($event, ind)'
			:key='el.charAt(ind)+ind'
			:autofocus='ind === 0'
			:placeholder='(ind + 1).toString()'
		/>
	</div>
</template>

<style scoped>
	.digit-box {
		height: 4rem;
		width: 2rem;
		border: 2px solid black;
		display: inline-block;
		border-radius: 5px;
		margin: 5px;
		padding: 15px;
		font-size: 3rem;
	}

	.digit-box:focus {
		outline: 3px solid black;
	}
</style>