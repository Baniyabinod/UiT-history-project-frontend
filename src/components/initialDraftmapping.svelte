
<!--
const LanguageStatusMap = {
    "0500": "Swedish",
    "0700": "Norwegian",
    "3300": "Finnish",
    "3520": "Sami",
    "9600": "Other",
    "9900": "No information",
  };

  const ethnicityMap = {
    "2100": "English",
    "3100": "German",
    "4050": "Norwegian",
    "4100": "Sami (lapp)",
    "4110": "Sami, nomads",
    "4120": "Sami, resident",
    "4200": "Mixed",
    "4210": "Norwegian-Sami",
    "4220": "Norwegian-Finnish",
    "4230": "Sami-Finnish",
    "5100": "Scandinavian",
    "5200": "Swedish",
    "5300": "Finnish",
    "5400": "Danish",
    "5500": "Icelander",
    "64109": "Bohemian",
    "6420": "Hungarian",
    "6430": "Polish or Mazurian",
    "6510": "Russian",
    "6520": "Russian Jew",
    "6730": "Italian",
    "7100": "American (Unspecified)",
    "9970": "Unknown",
    "9990": "No information",
  };
  const genderMap = {
    "0": "Other",
    "1": "Male",
    "2": "Female",
  };

  const residenceStatusMap = {
    "1": "Persons living in the house on census day",
    "2": "Persons living in the house but absent on census day",
    "3": "Temporary visitors on census day",
  };

  const familyPositionMap = {
    "01": "Household head",
    "02": "Spouse",
    "03": "Child, stepchild",
    "05": "Fostered, adopted child",
    "07": "Child-in-law",
    "08": "Grandchild",
    "09": "Parent, parent-in-law",
    "11": "Sibling, sibling-in-law",
    "13": "Other relatives",
    "14": "Servants",
    "15": "Apprentice",
    "16": "Lodgers",
    "17": "Paupers",
    "18": "Living with others",
    "20": "Visitors",
    "21": "Retired people with farm benefits",
    "23": "Others",
    "99": "Unknown",
    "00": "No information",
  };

  const maritalStatusMap = {
    "1": "Married, spouse present",
    "2": "Married, spouse absent",
    "3": "Separated",
    "4": "Divorced",
    "5": "Widowed",
    "6": "Single/never married",
    "9": "Unknown",
  };

  const primaryOccupationStatusMap = {
    "00": "Not given",
    "01": "Farmer, owner - ordinary sized farm",
    "02": "Proprietor of estate",
    "03": "House or building owner in town or city",
    "04": "Tenant farmer, lease-holder",
    "05": "Farmer, unspecified",
    "06": "Retired agriculturalists,remaining on farm with formal contract with (usually) children",
    "07": "Cottager with land",
    "08": "Cottager without land",
    "09": "Cottager (unspecified)",
    "10": "Lodger",
    "12": "Servant",
    "13": "Helps in the family",
    "20": "Owns and/or runs a business",
    "21": "Civil servant, high level",
    "22": "Civil servant, middle status",
    "23": "Civil servant, low status",
    "24": "Foreman",
    "25": "Guard, watchman, housekeeper, farm manager",
    "26": "Master (position gained through formal training and certified; owns own shop)",
    "27": "Other work leader (officer in the army, naval officer at court)",
    "37": "Journeyman",
    "38": "Workers, Day labourers",
    "39": "Apprentice",
    "40": "Other",
    "41": "Agent, Representative of companies or firms",
    "50": "Student, pupil",
    "51": "Rentier, “capitalist”",
    "52": "Pensioned",
    "53": "No longer working",
    "54": "Orphan, foster child",
    "55": "Pauper (fattiglem)",
    "56": "On municipal support",
    "57": "Receiving private or other support",
    "58": "Beggar",
    "59": "Prisoner",
  };

  const secondaryOccupationStatusMap = {
    "00": "Not given",
    "01": "Farmer, owner - ordinary sized farm",
    "02": "Proprietor of estate",
    "03": "House or building owner in town or city",
    "04": "Tenant farmer, lease-holder",
    "05": "Farmer, unspecified",
    "06": "Retired agriculturalists, remaining on farm with formal contract with (usually) children",
    "07": "Cottager with land",
    "08": "Cottager without land",
    "09": "Cottager (unspecified)",
    "10": "Lodger",
    "12": "Servant",
    "13": "Helps in the family",
    "20": "Owns and/or runs a business",
    "21": "Civil servant, high level",
    "22": "Civil servant, middle status",
    "23": "Civil servant, low status",
    "24": "Foreman",
    "25": "Guard, watchman, housekeeper, farm manager",
    "26": "Master (position gained through formal training and certified; owns own shop)",
    "27": "Other work leader (officer in the army, naval officer at court)",
    "37": "Journeyman",
    "38": "Workers, Day labourers",
    "39": "Apprentice",
    "40": "Other",
    "41": "Agent, Representative of companies or firms",
    "50": "Student, pupil",
    "51": "Rentier, “capitalist”",
    "52": "Pensioned",
    "53": "No longer working",
    "54": "Orphan, foster child",
    "55": "Pauper (fattiglem)",
    "56": "On municipal support",
    "57": "Receiving private or other support",
    "58": "Beggar",
    "59": "Prisoner",
  };

  const medicalConditionStatusMap = {
    "21": "Mental disease",
    "24": "Blind",
    "25": "Deaf",
    "26": "Deaf and dumb",
    "99": "Other",
    "00": "No information",
    x: "no reference",
  };

  const citizenshipStatusMap = {
    "404": "Norwegian",
    "405": "Swedish",
    "401": "Finnish",
    "400": "Danish",
    "4538": "German",
    "465": "Russian",
    "499": "Rest of Europe",
    "900": "Rest of World",
    "997": "Unknown",
    "999": "No information",
    x: "No reference",
  };

  const religionMap = {
    "1100": "Lutheran",
    "2300": "Baptist",
    "3100": "Roman Catholic",
    "3300": "Greek Orthodox",
    "4100": "Jewish",
    "4700": "Muslim",
    "4950": "Other Non-Christian",
    "5200": "Other Protestant",
    "6300": "Other Christian",
    "7100": "Buddhist",
    "8310": "Hindu",
    "8440": "Shinto",
    "8510": "Confucian",
    "8700": "Taoist",
    "8815": "Sikh",
    "8875": "Jain",
    "8890": "Zoroastrian",
    "9100": "Bahá'í",
    "9130": "Traditional African Religions",
    "9710": "Native American Religions",
    "9880": "No religion",
    "9990": "Unknown",
    "9998": "Not stated",
  };

  //this mapping is incomplete,we need to figure this out before displaying the data that's why used the function below to map the data
  const municipalityStatusMap = {
    "0101": "Halden",
    "0102": "Sarpsborg",
    "0103": "Fredrikstad",
    "0104": "Moss",
    "0105": "Horten",
    "0106": "Tønsberg",
    "0107": "Larvik",
    "0108": "Sandefjord",
    "0109": "Holmestrand",
    "0110": "Drammen",
    "0111": "Hvaler",
    "0112": "Aremark",
    "0113": "Marker",
    "0114": "Rømskog",
    "0115": "Trøgstad",
    "0116": "Spydeberg",
    "0117": "Askim",
    "0118": "Eidsberg",
    "0119": "Skiptvet",
    "0120": "Rakkestad",
    "0121": "Råde",
    "0122": "Rygge",
    "0123": "Våler",
    "0124": "Hobøl",
    "0125": "Vestby",
    "0126": "Ski",
    "0127": "Ås",
    "0128": "Frogn",
    "0129": "Nesodden",
    "0130": "Oppegård",
    "0131": "Bærum",
    "0132": "Asker",
    "0133": "Aurskog-Høland",
    "0134": "Oslo",
  };

  //as we get 659 kommunenr from the data, used this dummy data for now to test, have to replce this later with the actual mapping
  const kommunenrMap = {};
  const usedNames = new Set(); // To track unique names

  // Prepopulate some known values (optional)
  for (let i = 1; i <= 659; i++) {
    const key = i.toString().padStart(4, "0"); // Ensures the key is 4 digits
    kommunenrMap[key] = `Municipality ${i}`; // Default name
  }

  // Function to generate random names for kommune
  function generateRandomName(kommunenr) {
    // Fix variable name here
    const randomAdjectives = [
      "Sunny",
      "Green",
      "Quiet",
      "Busy",
      "Historic",
      "Modern",
      "Peaceful",
      "Scenic",
    ];
    const randomNouns = [
      "Haven",
      "Valley",
      "Hill",
      "Meadow",
      "Bay",
      "Shore",
      "Forest",
      "Grove",
    ];

    let randomName;

    // Generate random names until we find a unique one
    do {
      const adjective =
        randomAdjectives[Math.floor(Math.random() * randomAdjectives.length)];
      const noun = randomNouns[Math.floor(Math.random() * randomNouns.length)];
      randomName = `${adjective} ${noun} (Municipality ${kommunenr})`;
    } while (usedNames.has(randomName));

    usedNames.add(randomName);
    return randomName;
  }

  // Function to map population data
  function mapPopulationData(data) {
    return data.map((item) => ({
      kommunenr: item.kommunenr,
      //replace the name below with this one once you get the correct mapping and get rid of the kommunenrMap declaration and genrateRandomName function
      //name: municipalityStatusMap[item.kommunenr] || 'Unknown',
      name: kommunenrMap[item.kommunenr] || generateRandomName(item.kommunenr),
      Number_of_Persons: item.Number_of_Persons,
    }));
  }

  //mapping function for norwegian ethnicity name
  function mapNames(data) {
    return data.map((item) => ({
      "first name": item.fornavn,
      "last name": item.etternavn,
      // Add other fields if necessary
    }));
  }

  <main>
  <header>
    <h1>Population Census Dashboard</h1>
  </header>
  
  <div class="container">

    
    <section class="single-data-container">
      <SingleDataFetcher
        apiUrl="/api/total-population"
        title="Total Population in 1910 Census"
        dataKey="Total_Population"
      />
    </section>
    <section class="single-data-container">
      <SingleDataFetcher
        apiUrl="/api/total-children"
        title="Total children in 1910 Census"
        dataKey="Number_of_Children"
      />
    </section>
    <section class="single-data-container">
      <SingleDataFetcher
        apiUrl="/api/total-adults"
        title="Total adult(18-79) in 1910 Census"
        dataKey="Number_of_Adults"
      />
    </section>
    <section class="single-data-container">
      <SingleDataFetcher
        apiUrl="/api/total-elderly"
        title="Total elderly population(above 80) in 1910 Census"
        dataKey="Number_of_Elderly"
      />
    </section>

    <section class="card-container">
      <PopulationByAge
        apiUrl="/api/population-by-age-range"
        dataMap={genderMap}
        labelField="kjonn"
        valueField="Number_of_Persons"
      />
    </section>
    <section class="card-container">
      <SummaryCard
        apiUrl="/api/population-by-gender"
        title="Gender Distribution"
        dataMap={genderMap}
        labelField="kjonn"
        valueField="Number_of_Persons"
      />
    </section>
    <section class="card-container">
      <SummaryCard
        apiUrl="/api/population-by-residence-status"
        title="Population by residence status"
        dataMap={residenceStatusMap}
        labelField="bostatus"
        valueField="Number_of_Persons"
      />
    </section>

    <section class="table-container">
      <DataTable
        title="Names of people(norwegian ethnicity)"
        columns={["first name", "last name"]}
        apiUrl="/api/population-by-names"
        transformData={mapNames}
      />
    </section>

    <section class="table-container">
      <DataTable
        title="Population by Municipality"
        columns={["kommunenr", "name", "Number_of_Persons"]}
        apiUrl="/api/population-by-municipality"
        transformData={mapPopulationData}
      />
    </section>

  
    <section class="chart-container">
      <Chart
        apiUrl="/api/population-by-group"
        title="Population in chart "
        statusMap={religionMap}
        label="Number of Persons by Religion"
        labelField="religion"
        itemMapping={(item) => item.Number_of_Persons}
      />
    </section>
    
    <NewPopulationPyramid />
 
    <NewPopulationPyramidWithYears />
    <SinglePopulationByGender />
    <MaritalStatusByGender />
    

  
  </div>

  <footer class="footer">
    <p>&copy; UiT</p>
  </footer>
</main>

  -->