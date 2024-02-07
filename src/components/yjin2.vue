<template>
    <div class="card">
        <div class="pb-0 card-header mb-0">
            <h6>{{ title }}</h6>
        </div>
        <div class="p-3 card-body">
            <div class="chart">
                <canvas id="donut-chart" class="chart-canvas" height="300"></canvas>
            </div>
        </div>
    </div>
</template>

<script>
import Chart from "chart.js/auto";

export default {
    name: "donut-chart",

    props: {
        title: {
            type: String,
            default: "부서별 재직자 비율",
        },
    },

    mounted() {
        this.fetchData();
    },

    methods: {
        async fetchData() {
            try {
                const response = await fetch("https://hris-json-server.fly.dev/employees");
                if (!response.ok) {
                    throw new Error(`Failed to fetch data: ${response.status}`);
                }

                const employees = await response.json();
                const departmentCounts = this.calculateDepartmentCounts(employees);
                this.createDonutChart(departmentCounts);
            } catch (error) {
                console.error("Error fetching data:", error);
            }
        },

        calculateDepartmentCounts(employees) {
            return employees.reduce((counts, employee) => {
                counts[employee.department] = (counts[employee.department] || 0) + 1;
                return counts;
            }, {});
        },

        createDonutChart(departmentCounts) {
            const ctx = document.getElementById("donut-chart").getContext("2d");

            const labels = Object.keys(departmentCounts);
            const data = Object.values(departmentCounts);

            new Chart(ctx, {
                type: "doughnut",
                data: {
                    labels: labels,
                    datasets: [
                        {
                            data: data,
                            backgroundColor: ['#FF6384', '#36A2EB', '#FFCE56', '#8A2BE2', '#32CD32'],
                            borderWidth: 1,
                        },
                    ],
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            display: true,
                            position: 'bottom',
                        },
                    },
                },
            });
        },
    },
};
</script>