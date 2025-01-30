---
layout: post
title:  "Song list"
permalink: /song-list/
---

The Cantopop corpus consists of 105 songs from the period 2000–2020, with five songs from each year. The table is sortable by column headers.

Click on the Song ID to access transcriptions of the songs in Verovio Humdrum Viewer.

<div id="json-table"></div>

<style>
  table {
    margin-left: 50px;
    margin-right: 50px;
    border-collapse: collapse; /* Optional: Makes the table look cleaner */
    width: 90%; /* Optional: Adjust the width */
  }
  
  th, td {
    padding: 10px; /* Adjusts inner padding of cells */
    border: 1px solid #ddd; /* Optional: Adds border to cells */
    text-align: left; /* Optional: Aligns text to the left */
  }

  th:nth-child(1), td:nth-child(1) {
    width: 5%; /* First column width */
  }

  th:nth-child(2), td:nth-child(2) {
    width: 12%; /* Second column width */
  }

  th:nth-child(3), td:nth-child(3) {
    width: 5%; /* Third column width */
  }
  
  th:nth-child(4), td:nth-child(4) {
    width: 12%; 
  }

  th:nth-child(5), td:nth-child(5) {
    width: 12%; 
  }

  th:nth-child(6), td:nth-child(6) {
    width: 12%; 
  }

  th:nth-child(7), td:nth-child(7) {
    width: 12%; 
  }

  th:nth-child(8), td:nth-child(8) {
    width: 12%; 
  }
</style>

<script>
  let sortOrder = {}; // Store the current sort order for each column

  function fetchAndDisplayJSON() {
    const url = 'https://script.google.com/macros/s/AKfycbxOja89FodUqmV1yI1SFz18UN2k6BLR4VyzFryZ0vRmh4tuZ9TsdpAcJuYmLi4yXA/exec';

    // Fetch the JSON data
    fetch(url)
      .then(response => response.json())
      .then(data => {
        createTable(data);
      })
      .catch(error => console.error('Error fetching data:', error));
  }

  function createTable(data) {
    const container = document.getElementById('json-table');
    container.innerHTML = ''; // Clear any existing content

    const table = document.createElement('table');
    const headerRow = document.createElement('tr');

    // Get headers, excluding "VHV"
    const headers = Object.keys(data[0]).filter(header => header !== 'VHV');
    headers.forEach((header, index) => {
      const th = document.createElement('th');
      th.textContent = header;
      th.style.cursor = 'pointer';
      th.addEventListener('click', () => sortTable(table, data, header, index));
      headerRow.appendChild(th);
    });

    table.appendChild(headerRow);

    // Populate rows
    data.forEach(item => {
      const row = document.createElement('tr');
      headers.forEach(header => {
        const cell = document.createElement('td');
        if (header === 'ID ▼') {
          // Create hyperlink for the ID column using VHV value
          const link = document.createElement('a');
          link.href = item['VHV'];
          link.target = '_blank';
          link.textContent = item[header];
          cell.appendChild(link);
        } else {
          cell.textContent = item[header] !== undefined ? item[header] : '';
        }
        row.appendChild(cell);
      });
      table.appendChild(row);
    });

    container.appendChild(table);
  }

  function sortTable(table, data, column, index) {
    // Toggle sort order for this column
    sortOrder[column] = sortOrder[column] === 'asc' ? 'desc' : 'asc';

    // Sort data
    const sortedData = data.sort((a, b) => {
      const valA = a[column];
      const valB = b[column];

      if (valA < valB) return sortOrder[column] === 'asc' ? -1 : 1;
      if (valA > valB) return sortOrder[column] === 'asc' ? 1 : -1;
      return 0;
    });

    // Re-create the table with sorted data
    createTable(sortedData);
  }

  fetchAndDisplayJSON();
</script>
