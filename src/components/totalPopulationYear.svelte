<script>
    import { onMount } from "svelte";
    let selectedYear = "1910";
    let populationData = {};
    let error = "";
    const censusYears = ["1910", "1900", "1865"]; // Add more years as needed
    // Fetch population details based on the selected year
    async function fetchPopulationDetails() {
        try {
            const response = await fetch(
                `/api/population-depdendency/${selectedYear}`,
            );
            if (!response.ok) throw new Error("Failed to fetch data");
            const data = await response.json();
            populationData = data[0] || {};
        } catch (e) {
            error = e.message;
        }
    }
    // Fetch data on component mount
    onMount(fetchPopulationDetails);
    // Fetch new data when the user selects a different year
    function handleYearChange(event) {
        selectedYear = event.target.value;
        fetchPopulationDetails();
    }
</script>

<div class="container">
    <h2>Select Census Year</h2>
    <select on:change={handleYearChange}>
        {#each censusYears as year}
            <option value={year} selected={year === selectedYear}>{year}</option
            >
        {/each}
    </select>
    {#if error}
        <p class="error">Error: {error}</p>
    {:else}
        <div class="population-details">
            <div class="detail-card">
                <h3>Total Population</h3>
                <p>{populationData.Total_Population || "N/A"}</p>
            </div>
            <div class="detail-card">
                <h3>Total Children (0-14)</h3>
                <p>{populationData.Total_Children || "N/A"}</p>
            </div>
            <div class="detail-card">
                <h3>Total Elderly (65+)</h3>
                <p>{populationData.Total_Elderly || "N/A"}</p>
            </div>
            <div class="detail-card">
                <h3>Total Working (15-64)</h3>
                <p>{populationData.Total_Working || "N/A"}</p>
            </div>
            <div class="detail-card">
                <h3>Dependency Ratio</h3>
                <p>{populationData.Dependency_Ratio || "N/A"} per 100</p>
            </div>
        </div>
    {/if}
</div>

<style>
    .container {
        max-width: 700px;
        margin: 20px auto;
        padding: 20px;
        background-color: #f4f4f9;
        border-radius: 10px;
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        text-align: center;
    }
    select {
        padding: 10px;
        margin-bottom: 20px;
        font-size: 1em;
        border-radius: 5px;
        border: 1px solid #ccc;
        width: 80%;
        max-width: 300px;
    }
    .population-details {
        display: flex;
        flex-wrap: wrap;
        justify-content: space-around;
        gap: 15px;
    }
    .detail-card {
        background-color: white;
        border-radius: 8px;
        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
        padding: 15px;
        width: 45%;
        max-width: 200px;
        text-align: center;
    }
    .detail-card h3 {
        margin: 0 0 10px;
        font-size: 1.1em;
    }
    .detail-card p {
        margin: 0;
        font-size: 1.5em;
        color: #333;
    }
    .error {
        color: red;
        font-weight: bold;
    }
</style>
