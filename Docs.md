# Energy Data Assignment

This documentation outlines the core functions of the Energy Data Assignment app, highlighting key features and functionalities.

### MainPage.vue

- **Functionality**:
  - Provides input fields to select start and end dates.
  - Filters the table and chart data based on the selected date range.
  - Provides checkboxes for each country to show/hide its corresponding data.
  - Updates the table and chart dynamically based on the selected checkboxes.
  - Provides a button to clear the selected dates.
- **Configuration**:
  - The component is mounted with the data from the JSON file.
  - clearRanges method is used to set the array that we are displaying the table and chart to an empty array. Also sets the start and end dates to null.
  - handleStartDate() and handleEndDate() methods are used to set the start and end dates based on the user input. They are using .filter method to filter the data and server them to the table and chart respectively.
  - handleCountryToggle(country): Toggles the visibility of the corresponding country's data in the table and chart. By receiving the country name, the parent component will update the app with the correct country data and visibility state.
  - created(): We are initializing the app by setting the sliced_data array to the original data received from the backend/local file. We then iterate over the data and push each record in the corresponding country's price array(chart configuration) and finally storing all the dates in the allDates array for the chart.
- **Components**: DataTable.vue, Line Chart, NavBar.vue, Spinner.vue

### DataTable.vue

- **Functionality**:
  - Parses the provided JSON file containing timeseries datasets.
  - Renders the data in a table format with columns for date, time, and prices for each country (Germany, Greece, France).
- **Props**:
  - filteredData: Array of objects representing timeseries data for each country.
  - greekFlag, frenchFlag, germanFlag: Boolean values representing Toggling state for each country.
- **Computed Properties**:
  - formattedData: Returns the filtered data with formatted date and time columns.
  - Data is being received as: 2024-02-01T00:00:00 by formatting the data to: 2024-02-01 00:00:00 we have a visibly appealing and more understood display for the user.
- **Data Source**: [timeseries.json](data/timeseries.json)

### Line Chart

- **Description**: Visualizes timeseries data trends over time using a line chart.
- **Functionality**:
  - Utilizes Chart.js library to generate a line chart.
  - Represents each country's prices over time as separate lines on the chart.
- **Configuration**:

  - Data is being received as: 2024-02-01T00:00:00 by formatting the data to: 2024-02-01 00:00:00
  - We are passing two props that configure the chart: data and options.
  - data: Is a computed property named chartData that returns a labels and datasets array for the chart. With the use of conditionals to display the correct data for each country based on the toggling state and date range from the NavBar component.
  - options: Configuration object needed by the Chart.js library to generate the chart. Here we are using the responsive option to make the chart responsive to the window size and the maintainAspectRatio option to maintain the aspect ratio of the chart.

- **Components**: MainPage.vue (utilizes Vue Chart.js)

### NavBar.vue

- **Description**: Allows users to filter data based on selected start and end dates. Enables users to toggle visibility of specific datasets (countries) in the table and chart.
- **Functionality**:
  - Provides input fields to select start and end dates.
  - Filters the table and chart data based on the selected date range.
  - Provides checkboxes for each country to show/hide its corresponding data.
  - Updates the table and chart dynamically based on the selected checkboxes.
- **Configuration**:
  - Date configuration: Two input elements for selecting start and end dates. By changing the date in one input there is an event emitted to the parent component that will update the start or end date accordingly. Which in turn will update the app with the correct range of dates we want to display.
  - Country configuration: Three checkboxes for each country. These checkboxes allow users to toggle visibility of the corresponding data in the table and chart. By emitting an event with the country name and the visibility state, the parent component will update the app with the correct country data and visibility state.
  - Clear Dates configuration: A button that will clear the selected start and end dates by emitting an event that will trigger the parent component to clear the dates.
  - CountryCheckbox: shared Component that receives a flag and country name and emits an event with the country name and the visibility state. Can be resuable in other components.
- **Components**: NavBar.vue, CountryCheckbox.vue

### Spinner.vue

- **Description**: Indicates loading state of the application.
- **Functionality**:
  - Displays a spinner animation while data is being loaded or processed.
  - loading data property is used to toggle the spinner on or display the section with the data.
- **Components**: Spinner.vue

---

## References

- [Vue.js](https://vuejs.org/)
- [Chart.js](https://www.chartjs.org)
- [Vite](https://vitejs.dev)
- [TailwindCSS](https://tailwindcss.com)
