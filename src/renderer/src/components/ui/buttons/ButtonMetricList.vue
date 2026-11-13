<!--
Component for a metric list button. This button is used in the collapsed metric list of a memory graph or metric section.

Author: Tobias Stuckenberger
-->
<template>
    <div class="shrink-0">
        <a class="flex h-full flex-row justify-between space-x-2 rounded bg-primary p-2 py-1 text-background">
            <p class="line-clamp-2 min-w-12 self-center">{{ data.display_name }}</p>
            <div class="flex grow flex-row items-center justify-end text-sm">
                <p v-if="comparisonValue !== undefined" class="text-center">
                    {{ formatMetricValue(data, comparisonValue, comparisonSecondaryValue) }}
                </p>
                <p v-if="comparisonValue !== undefined" class="px-1">vs</p>
                <p class="text-center" :class="comparisonClass">
                    {{ formatMetricValue(data, value, secondaryValue) }}
                </p>
            </div>
        </a>
    </div>
</template>
<script setup>
import { getMetricsData, formatMetricValue } from '../../../utils/formatters';
import { computed } from 'vue';

const props = defineProps({
    metric: String,
    value: [Number, String],
    comparisonValue: [Number, String],
    hint: String,
    secondaryValue: Number,
    comparisonSecondaryValue: Number
});

const canCompare = computed(() => Number.isFinite(props.value) && Number.isFinite(props.comparisonValue));

const comparisonClass = computed(() => {
    if (!canCompare.value || props.value === props.comparisonValue)
        return '';

    const isBetter = data.lower_better
        ? props.value < props.comparisonValue
        : props.value > props.comparisonValue;

    return isBetter ? 'text-green-300' : 'text-red-300';
});

const data = getMetricsData(props.metric);
</script>
