<script>
  // @ts-nocheck

  import NewPopulationPyramidWithYears from "./components/dynamicYear.svelte";
  import SinglePopulationByGender from "./components/singlePopulationByGender.svelte";
  import MaritalStatusByGender from "./components/maritalStatusByGender.svelte";
  import NewPopulationPyramidWithYearsAndCity from "./components/dynamicYearAndCity.svelte";
  import TotalPopulation from "./components/totalPopulationYear.svelte";
  import PopulationByOccupation from "./components/populationByOccupation.svelte";
  import { language } from "./languageStore";
  import { translations } from "./translation";
  let currentLanguage = "en";
  language.subscribe((value) => (currentLanguage = value));
</script>

<main>
  <header>
    <h1>{translations[currentLanguage].header}</h1>
  </header>
  <div class="hero">
    <p>{translations[currentLanguage].hero}</p>
  </div>
   <div class="language-container">
    <p>{translations[currentLanguage].languageSelectionDescription}</p>
    <button on:click={() => language.set('en')}>English</button>
    <button on:click={() => language.set('no')}>Norwegian</button>
    <button on:click={() => language.set('sa')}>Sami</button>
  </div>
  <div class="container">
    <div class="section">
      <h2>{translations[currentLanguage].totalPopulationTitle}</h2>
      <p>{translations[currentLanguage].totalPopulationTitleDescription}</p>
      <TotalPopulation />
    </div>
    <div class="section">
      <h2>{translations[currentLanguage].occupationPopulationTitle}</h2>
      <p>{translations[currentLanguage].occupationPopulationTitleDescription}</p>
      <PopulationByOccupation />
    </div>
    <div class="section">
      <h2>{translations[currentLanguage].ageGroupTitle}</h2>
      <p>{translations[currentLanguage].ageGroupDescription}</p>
      <NewPopulationPyramidWithYears />
    </div>
    <div class="section">
      <h2>{translations[currentLanguage].ageGroupByCityTitle}</h2>
      <p>{translations[currentLanguage].ageGroupByCityTitleDescription}</p>
      <NewPopulationPyramidWithYearsAndCity />
    </div>
    <div class="section">
      <h2>{translations[currentLanguage].maritalStatusTitle}</h2>
      <p>{translations[currentLanguage].maritalStatusDescription}</p>
      <MaritalStatusByGender />
    </div>
    <div class="section">
      <h2>{translations[currentLanguage].singlePopulationTitle}</h2>
      <p>{translations[currentLanguage].singlePopulationDescription}</p>
      <SinglePopulationByGender />
    </div>

  
  </div>
  <footer>
    <p>&copy; UiT / photo credit : Mostphoto</p>
  </footer>
</main>

<style>
  main {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 0;
    color: #333;
  }
  header {
    background-image: url("./assets/sample1.jpg");
    background-size: cover;
    background-position: center;
    color: white;
    padding: 60px 20px;
    text-align: center;
    position: relative;
  }
  .language-container {
    display: flex;
    justify-content: center;
    margin: 20px 0;
  }
  .language-container button {
    margin: 0 10px;
    padding: 10px 20px;
    font-size: 1em;
    cursor: pointer;
    border: none;
    border-radius: 5px;
    background-color: #f4f4f9;
    color: #333;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  }
  .language-container button:hover {
    background-color: #ddd;
  }

  h1 {
    margin: 0;
    font-size: 2.5em;
    text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
  }
  .hero {
    padding: 40px 20px;
    background-color: rgba(0, 0, 0, 0.5); /* Semi-transparent background */
    color: white;
    text-align: center;
    position: relative;
    top: -20px; /* Move up to overlap with header */
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    border-radius: 8px;
  }
  .hero p {
    max-width: 600px;
    margin: 0 auto;
    font-size: 1.2em;
    line-height: 1.6;
    color: #f4f4f9;
  }
  .container {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 10px;
    background-color: #f4f4f9;
  }
  .section {
    width: 80%;
    margin: 20px 0;
    padding: 20px;
    background-color: white;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    border-radius: 8px;
  }
  footer {
    text-align: center;
    padding: 20px;
    background-color: #333;
    color: white;
  }
</style>
