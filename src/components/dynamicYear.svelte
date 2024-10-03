<script>
    import { onMount } from "svelte";
    import Chart from "chart.js/auto";
    let data = [];
    let error = "";
    let chartContainer;
    let chartInstance;
    let selectedYear = '1910'; // Default selected year
    let censusYears = ['1910', '1900', '1865']; // Add more census years as needed
    // Fetch data based on the selected year
    async function fetchData() {
      try {
        const response = await fetch(`/api/population-by-gender-group/${selectedYear}`);
        if (!response.ok) throw new Error('Network response was not ok.');
        data = await response.json();
        renderChart();
      } catch (e) {
        error = e.message;
      }
    }
    // Render the chart
    function renderChart() {
      const ctx = chartContainer.getContext('2d');
      // Destroy existing chart instance if it exists
      if (chartInstance) {
        chartInstance.destroy();
      }
      // Sort data by age group
      data.sort((a, b) => {
        const ageA = parseInt(a.age_group.split("-")[0]);
        const ageB = parseInt(b.age_group.split("-")[0]);
        return ageA - ageB;
      });
      const ageGroups = [...new Set(data.map((item) => item.age_group))].reverse(); // Reverse the order
      const colorPalette = ["#FF5733", "#33FF57"];
      const datasets = [
        {
          label: 'Male',
          data: ageGroups.map(group => {
            const males = data.find(d => d.age_group === group && d.gender === '1') || { number_of_people: 0 };
            return -males.number_of_people; // Negative for left side
          }),
          backgroundColor: colorPalette[0],
        },
        {
          label: 'Female',
          data: ageGroups.map(group => {
            const females = data.find(d => d.age_group === group && d.gender === '2') || { number_of_people: 0 };
            return females.number_of_people; // Positive for right side
          }),
          backgroundColor: colorPalette[1],
        }
      ];
      chartInstance = new Chart(ctx, {
        type: 'bar',
        data: {
          labels: ageGroups,
          datasets: datasets,
        },
        options: {
          indexAxis: 'y', // Horizontal bar chart
          responsive: true,
          scales: {
            x: {
              stacked: true,
              grid: {
                color: 'rgba(220, 220, 220, 0.3)', // Subtle grid lines
              },
              ticks: {
                callback: function(value) {
                  return Math.abs(value); // Show positive values on x-axis
                },
                font: {
                  size: 12, // Font size for x-axis labels
                },
                color: '#666', // Color for x-axis labels
              },
              title: {
                display: true,
                text: 'Number of People', // Title for x-axis
                font: {
                  size: 14,
                  weight: 'bold',
                },
                color: '#333',
              },
            },
            y: {
              stacked: true,
              grid: {
                color: 'rgba(220, 220, 220, 0.3)', // Subtle grid lines
              },
              ticks: {
                font: {
                  size: 12, // Font size for y-axis labels
                },
                color: '#666', // Color for y-axis labels
              },
              title: {
                display: true,
                text: 'Age Groups', // Title for y-axis
                font: {
                  size: 14,
                  weight: 'bold',
                },
                color: '#333',
              },
            },
          },
          plugins: {
            legend: {
              position: 'top',
            },
            tooltip: {
              callbacks: {
                label: function (tooltipItem) {
                  return `Number of People: ${Math.abs(tooltipItem.raw)}`;
                },
              },
            },
          },
        },
      });
    }
    // Fetch data on mount and when year changes
    onMount(fetchData);
    // Fetch new data when the user selects a different year
    function handleYearChange(event) {
      selectedYear = event.target.value;
      fetchData();
    }
  </script>
  <!-- <h1>Male and Female population by Age Group</h1>-->
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