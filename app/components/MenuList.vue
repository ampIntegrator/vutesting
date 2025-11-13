<script setup>
const props = defineProps({
    context: {
        type: String,
        required: true // 'bottom' ou 'sticky'
    }
})

const emit = defineEmits(['menuClick'])

const menuItems = [
    'Pour vous',
    'Le Végétal &<br>les métiers du futur',
    'L\'interprofession<br>VALHOR',
    'Valoriser<br>la filière',
    'Filière<br>durable',
    'Le marché<br>du Végétal',
    'Faire pousser<br>l\'excellence'
]

const handleClick = (index, event) => {
    emit('menuClick', props.context, index, event)
}
</script>

<template>
    <ul class="ulWithSub">
        <li v-for="(item, index) in menuItems" :key="index" @click="handleClick(index, $event)">
            <span v-html="item"></span>
        </li>
    </ul>
</template>


<style lang="scss" scoped>
ul {
    margin: 0 auto;
    display: flex;
    align-items: center;
    transition: $trans;

    li {
        flex-grow: 1;
        display: flex;
        align-items: center;
        justify-content: start;
        transition: $trans;
        text-align: center;


        span {
            width: 100%;
            color: $ink;
            font-weight: 500;
                      line-height: 1.25;
          display: flex;
          align-items: center;
          justify-content: center;
        }

        &.active {
            background-color: $light;

            span {
                color: $main
            }
        }
    }
}
</style>