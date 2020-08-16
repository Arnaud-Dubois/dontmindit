<template>
    <div class="flex cursor-pointer" @click="$emit('click')">
        <div
            :class="[isChecked ? 'bg-color-prime ripple' : 'bg-gray-200']"
            class="task-checkbox rounded-full transition-all duration-300 ease-in-out"
        >
        </div>
        <slot></slot>
    </div>
</template>

<script>
export default {
    name: 'task-checkbox',
    props: {
        isChecked: Boolean
    }
}
</script>

<style lang="css">
    .task-checkbox {
        width: 2rem;
        height: 2rem;
        
    }
    .task-checkbox::before {
        position: absolute;
        display: block;
        content: "";
        width: 8px;
        height: 14px;
        opacity: 0;
        border: 2px solid white;
        border-top-width: 0;
        border-left-width: 0;
        transform: translate(11px, 6px)rotate(45deg);
    }
    .task-checkbox.ripple::before {
        animation: validation 0.4s forwards ease-in-out;
    }
    .task-checkbox::after {
        display: block;
        content: "";
        width: 100%;
        height: 100%;
        opacity: 0;
        border-radius: 50%;
        pointer-events: none;
        border: 2px solid #3609fb;
    }
    .task-checkbox.ripple::after {
        animation: ripple 0.4s forwards ease-in;
    }
    .bg-color-prime {
        background-color: #3609fb;
    }
    .border-color-prime {
        border-color: #3609fb;
    }

    @keyframes validation {
        from {
            margin-top: -14px;
            opacity: 0;
        }
        to {
            margin-top: 0px;
            opacity: 1;
        }
    }
    @keyframes ripple {
        from {
            transform: scale(1);
            opacity: 1;
        }
        to {
            transform: scale(3);
            opacity: 0;
        }
    }

</style>