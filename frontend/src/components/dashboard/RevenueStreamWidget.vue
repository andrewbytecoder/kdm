<script setup lang="ts">
import { useLayout } from '../../layout/composables/layout';
import { onMounted, ref, watch, type Ref } from 'vue';

// 定义图表数据类型
interface ChartData {
    labels: string[];
    datasets: Array<{
        type: string;
        label: string;
        backgroundColor: string;
        data: number[];
        barThickness: number;
        borderRadius?: {
            topLeft: number;
            topRight: number;
        };
        borderSkipped?: boolean;
    }>;
}

// 定义图表选项类型
interface ChartOptions {
    maintainAspectRatio: boolean;
    aspectRatio: number;
    scales: {
        x: {
            stacked: boolean;
            ticks: {
                color: string;
            };
            grid: {
                color: string;
                borderColor: string;
            };
        };
        y: {
            stacked: boolean;
            ticks: {
                color: string;
            };
            grid: {
                color: string;
                borderColor: string;
                drawTicks: boolean;
            };
        };
    };
}

const { getPrimary, getSurface, isDarkTheme } = useLayout();

const chartData: Ref<ChartData | null> = ref(null);
const chartOptions: Ref<ChartOptions | null> = ref(null);

function setChartData(): ChartData {
    const documentStyle = getComputedStyle(document.documentElement);

    return {
        labels: ['Q1', 'Q2', 'Q3', 'Q4'],
        datasets: [
            {
                type: 'bar',
                label: 'Subscriptions',
                backgroundColor: documentStyle.getPropertyValue('--p-primary-400'),
                data: [4000, 10000, 15000, 4000],
                barThickness: 32
            },
            {
                type: 'bar',
                label: 'Advertising',
                backgroundColor: documentStyle.getPropertyValue('--p-primary-300'),
                data: [2100, 8400, 2400, 7500],
                barThickness: 32
            },
            {
                type: 'bar',
                label: 'Affiliate',
                backgroundColor: documentStyle.getPropertyValue('--p-primary-200'),
                data: [4100, 5200, 3400, 7400],
                borderRadius: {
                    topLeft: 8,
                    topRight: 8
                },
                borderSkipped: true,
                barThickness: 32
            }
        ]
    };
}

function setChartOptions(): ChartOptions {
    const documentStyle = getComputedStyle(document.documentElement);
    const borderColor = documentStyle.getPropertyValue('--surface-border');
    const textMutedColor = documentStyle.getPropertyValue('--text-color-secondary');

    return {
        maintainAspectRatio: false,
        aspectRatio: 0.8,
        scales: {
            x: {
                stacked: true,
                ticks: {
                    color: textMutedColor
                },
                grid: {
                    color: 'transparent',
                    borderColor: 'transparent'
                }
            },
            y: {
                stacked: true,
                ticks: {
                    color: textMutedColor
                },
                grid: {
                    color: borderColor,
                    borderColor: 'transparent',
                    drawTicks: false
                }
            }
        }
    };
}

watch([getPrimary, getSurface, isDarkTheme], () => {
    chartData.value = setChartData();
    chartOptions.value = setChartOptions();
});

onMounted(() => {
    chartData.value = setChartData();
    chartOptions.value = setChartOptions();
});
</script>

<template>
    <div class="card">
        <div class="font-semibold text-xl mb-4">Revenue Stream</div>
        <Chart type="bar" :data="chartData" :options="chartOptions" class="h-80" />
    </div>
</template>