<script>
    import { onMount } from "svelte";
    import Chart from "chart.js/auto";
    let selectedYear = "1910";
    let data = [];
    let error = "";
    let chartContainer;
    let chartInstance;
    const censusYears = ["1910", "1900", "1865"];
    const occupationMap = {
        "06110": "Doctor",
        "06400": "Pharmacist",
        "07320": "Midwife",
    };
    
    async function fetchData() {
        try {
            const response = await fetch(
                `/api/population-by-occupation/${selectedYear}`,
            );
            if (!response.ok) throw new Error("Failed to fetch data");
            data = await response.json();
            console.log("Data:", data); // Log the parsed JSON data
            renderChart();
        } catch (e) {
            error = e.message;
        }
    }
    function renderChart() {
        const ctx = chartContainer.getContext("2d");
        if (chartInstance) {
            chartInstance.destroy();
        }
        let ageGroups = [...new Set(data.map((item) => item.age_group))].sort(
            (a, b) => {
                const ageA = parseInt(a.split("-")[0]);
                const ageB = parseInt(b.split("-")[0]);
                return ageA - ageB;
            },
        );
        ageGroups = ageGroups.reverse();
        const datasets = Object.keys(occupationMap).flatMap((code) => [
            {
                label: `${occupationMap[code]} (Male)`,
                data: ageGroups.map((group) => {
                    const males = data.filter(
                        (d) =>
                            d.age_group === group &&
                            d.gender === "1" &&
                            d.occ_napp_hisco === code,
                    );
                    return -males.reduce(
                        (sum, item) => sum + item.number_of_people,
                        0,
                    );
                }),
                backgroundColor: "#FF5733",
            },
            {
                label: `${occupationMap[code]} (Female)`,
                data: ageGroups.map((group) => {
                    const females = data.filter(
                        (d) =>
                            d.age_group === group &&
                            d.gender === "2" &&
                            d.occ_napp_hisco === code,
                    );
                    return females.reduce(
                        (sum, item) => sum + item.number_of_people,
                        0,
                    );
                }),
                backgroundColor: "#33FF57",
            },
        ]);
        chartInstance = new Chart(ctx, {
            type: "bar",
            data: {
                labels: ageGroups,
                datasets: datasets,
            },
            options: {
                indexAxis: "y",
                responsive: true,
                scales: {
                    x: {
                        stacked: true,
                        grid: {
                            color: "rgba(220, 220, 220, 0.3)",
                        },
                        ticks: {
                            callback: (value) => Math.abs(Number(value)),
                            font: {
                                size: 12,
                            },
                            color: "#666",
                        },
                        title: {
                            display: true,
                            text: "Number of People",
                            font: {
                                size: 14,
                                weight: "bold",
                            },
                            color: "#333",
                        },
                    },
                    y: {
                        stacked: true,
                        grid: {
                            color: "rgba(220, 220, 220, 0.3)",
                        },
                        ticks: {
                            font: {
                                size: 12,
                            },
                            color: "#666",
                        },
                        title: {
                            display: true,
                            text: "Age Groups",
                            font: {
                                size: 14,
                                weight: "bold",
                            },
                            color: "#333",
                        },
                    },
                },
                plugins: {
                    legend: {
                        position: "top",
                    },
                    tooltip: {
                        callbacks: {
                            label: (tooltipItem) =>
                                `Number of People: ${Math.abs(Number(tooltipItem.raw))}`,
                        },
                    },
                },
            },
        });
    }
    onMount(fetchData);
    function handleYearChange(event) {
        selectedYear = event.target.value;
        fetchData();
    }
</script>

<div>
    <label for="yearSelect">Select Census Year: </label>
    <select id="yearSelect" on:change={handleYearChange}>
        {#each censusYears as year}
            <option value={year} selected={year === selectedYear}>{year}</option
            >
        {/each}
    </select>
</div>
{#if error}
    <p>Error: {error}</p>
{:else}
    <canvas bind:this={chartContainer}></canvas>
{/if}
