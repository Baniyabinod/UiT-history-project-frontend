<script>
    import { onMount } from "svelte";
    import Chart from "chart.js/auto";
    let data = [];
    let error = "";
    let chartContainer;
    let chartInstance;
    let selectedYear = "1910"; // Default selected year
    let censusYears = ["1910", "1900", "1865"]; // Add more census years as needed
    const maritalStatusMap = {
      "Married": "Married",
      "Divorced": "Divorced",
      "Widowed": "Widowed",
      "Single/never married": "Single/never married",
    };
    const genderMap = {
      "1": "Male",
      "2": "Female",
    };
    const colorPalette = {
      "Married Male": "#6333ff",
      "Married Female": "#6333ff",
      "Divorced Male": "#000000",
      "Divorced Female": "#000000",
      "Widowed Male": "#FF8C57",
      "Widowed Female": "#FF8C57",
      "Single/never married Male": "#FF5733",
      "Single/never married Female": "#33FF57",
    };
    // Fetch data based on the selected year
    async function fetchData() {
      try {
        const response = await fetch(`/api/population-by-marital-group-based-on-gender/${selectedYear}`);
        if (!response.ok) {
          console.error("Network response was not ok:", response.status, response.statusText);
          throw new Error("Network response was not ok.");
        }
        const contentType = response.headers.get("content-type");
        if (!contentType || !contentType.includes("application/json")) {
          console.error("Received non-JSON response:", contentType);
          throw new Error("Received non-JSON response");
        }
        data = await response.json();
        console.log("Data:", data); // Log the parsed JSON data
        renderChart();
      } catch (e) {
        console.error("Fetch error:", e);
        error = e.message;
      }
    }
    // Render the chart
    function renderChart() {
      const ctx = chartContainer.getContext("2d");
      if (chartInstance) {
        chartInstance.destroy();
      }
      // Sort age groups numerically
      let ageGroups = [...new Set(data.map((item) => item.age_group))].sort((a, b) => {
        const ageA = parseInt(a.split("-")[0]);
        const ageB = parseInt(b.split("-")[0]);
        return ageA - ageB;
      });
      // Reverse age groups to display younger age groups at the bottom
      ageGroups = ageGroups.reverse();
      const datasets = Object.keys(maritalStatusMap)
        .map((status) => {
          return [
            {
              label: `${status} (Male)`,
              data: ageGroups.map((group) => {
                const males = data.filter(
                  (d) =>
                    d.age_group === group &&
                    d.gender === "1" &&
                    d.marital_status === status,
                );
                return -males.reduce((sum, item) => sum + item.number_of_people, 0); // Negative for left side
              }),
              backgroundColor: colorPalette[`${status} Male`],
            },
            {
              label: `${status} (Female)`,
              data: ageGroups.map((group) => {
                const females = data.filter(
                  (d) =>
                    d.age_group === group &&
                    d.gender === "2" &&
                    d.marital_status === status,
                );
                return females.reduce((sum, item) => sum + item.number_of_people, 0); // Positive for right side
              }),
              backgroundColor: colorPalette[`${status} Female`],
            },
          ];
        })
        .flat();
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
                callback: function (value) {
                  return Math.abs(Number(value));
                },
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
                label: function (tooltipItem) {
                  return `Number of People: ${Math.abs(Number(tooltipItem.raw))}`;
                },
              },
            },
          },
        },
      });
    }
    // Fetch data on mount
    onMount(fetchData);
    // Fetch new data when the user selects a different year
    function handleYearChange(event) {
      selectedYear = event.target.value;
      fetchData();
    }
  </script>
  <!--<h1>Male and Female Population by Marital Status</h1>-->
  <!-- Dropdown for selecting census year -->
  <div>
    <label for="yearSelect">Select Census Year: </label>
    <select id="yearSelect" on:change={handleYearChange}>
      {#each censusYears as year}
        <option value={year} selected={year === selectedYear}>{year}</option>
      {/each}
    </select>
  </div>
  {#if error}
    <p>Error: {error}</p>
  {:else}
    <canvas bind:this={chartContainer}></canvas>
  {/if}