<template>
	<div ref="defaultRef" class="relative" @mouseenter="onShow" @mouseleave="onHidden">
		<slot name="trigger" />
		<transition name="select-animation">
			<div
				v-show="visible && !disabled"
				style="transition-timing-function: ease"
				class="absolute flex bottom-4 bg-white border border-gray-100 z-10 rounded-sm shadow-lg min-w-max p-1 text-[13px] popover"
				:style="{ bottom: `${defaultWidth + 14}px` }"
			>
				<slot />
			</div>
		</transition>
	</div>
</template>

<script setup lang="ts">
// trigger 是一个插槽，用于触发显示 popover 的元素
// default 是一个插槽，用于显示 popover 的内容
defineProps({
	disabled: {
		type: Boolean,
		required: false,
		default: false,
	},
})

const visible = ref(false)
const emits = defineEmits<{
	(e: 'show'): void
}>()
const defaultWidth = ref(0)
const defaultRef = ref()

const timer = ref<NodeJS.Timer | null>(null)

const onShow = () => {
	timer.value && clearTimeout(timer.value)
	visible.value = true
	emits('show')
}

const onHidden = () => {
	// timer.value = setTimeout(() => {
	visible.value = false
	// }, 200)
}

onMounted(() => {
	// 初始化默认宽度
	defaultWidth.value = defaultRef.value?.getBoundingClientRect().height
})
</script>

<style lang="scss" scoped>
.popover {
	left: 50%;
	color: #333;
	transform: translateX(-50%);
	&::after {
		content: '';
		position: absolute;
		left: 50%;
		transform: translateX(-50%);
		top: 100%;
		z-index: 11;
		width: 0px;
		height: 0px;
		border: 6px solid transparent;
		border-top: 7px solid #fff;
	}
}
.select-animation-enter-active,
.select-animation-leave-active {
	transition: all 1s ease;
}

.select-animation-enter-from,
.select-animation-leave-to {
	opacity: 0;
	// transform: translateY(-10px);
}

.index-nav-group {
    padding: 20px; // 添加内边距
    background-color: #f9f9f9; // 背景色
    border-radius: 8px; // 圆角
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1); // 整体阴影
}

@media (max-width: 768px) {
    .index-nav-group-content-item {
        flex-direction: column; // 在小屏幕上垂直排列
        align-items: center; // 居中对齐
    }
}

.index-nav-group-content-item {
    border: 1px solid #e0e0e0; // 添加边框
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); // 添加阴影
    transition: transform 0.3s ease, background-color 0.3s ease; // 平滑过渡
}

.index-nav-group-content-item:hover {
    transform: translateY(-4px); // 悬停时上移
    background-color: rgba(0, 149, 255, 0.1); // 悬停时背景色变化
}

.index-nav-group-content-item-name {
    font-size: 16px; // 增大字体
    font-weight: bold; // 加粗字体
    color: #333; // 深色文本
}

.index-nav-group-content-item-desc {
    font-size: 12px; // 保持小字体
    color: #757575; // 使用更柔和的颜色
}
</style>
