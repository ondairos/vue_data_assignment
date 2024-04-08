# Energy Data

This project showcases the use of Vue.js to create a data visualization application that allows users to view and filter energy price data for three countries in a table and chart format.

### Dependencies

To run the project, you will need to install the following dependencies by executing the following commands:

- npm install
- npm run dev

### Deployed Application

- [https://vue-data-assignment.netlify.app/](https://vue-data-assignment.netlify.app/)

### Tools

- [Vue.js](https://vuejs.org/) JavaScript framework.
- [Chart.js](https://www.chartjs.org) JavaScript charting library.
- [Vite](https://vitejs.dev) Build tool that provides fast development server and optimized builds.
- [TailwindCSS](https://tailwindcss.com) CSS framework that provides pre-built components and utility classes.

### Data Representation

- A JSON file containing three timeseries datasets with timestamps and values is provided.

## Features

### Data Visualization

- The data is visualized using a line chart to help users understand trends over time, utilizing the Chart.js library.
- The data is represented in a table to display the timeseries values for each country.

### Date Range

- A date range picker allows users to filter data in the table and chart based on selected start and end dates.
- Clear button is provided to reset the date range to the default values.

### Interactive Data Control

- Checkboxes for each timeseries dataset have been added, enabling users to show or hide specific countries in the table and chart.

## Project Setup

### Package Information

```json
{
  "name": "vue_data_assignment",
  "private": true,
  "version": "0.0.0",
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview"
  },
  "dependencies": {
    "autoprefixer": "^10.4.18",
    "chart.js": "^4.4.2",
    "postcss": "^8.4.35",
    "tailwindcss": "^3.4.1",
    "vue": "^3.4.19",
    "vue-chartjs": "^5.3.0"
  },
  "devDependencies": {
    "@vitejs/plugin-vue": "^5.0.4",
    "vite": "^5.1.4"
  }
}
```

## Created by: [Ioannis Kantiloros](https://github.com/ondairos)
