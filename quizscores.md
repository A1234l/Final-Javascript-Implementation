<head>
    <!-- load jQuery and DataTables syle and scripts -->
    <link rel="stylesheet" type="text/css" href="https://cdn.datatables.net/1.10.25/css/jquery.dataTables.min.css">
    <script type="text/javascript" language="javascript" src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
    <script type="text/javascript" language="javascript" src="https://cdn.datatables.net/1.10.25/js/jquery.dataTables.min.js"></script>
</head>

<table id="JTable" class="table" style="width:100%">
    <thead id="JHead">
        <tr>
            <th>Username</th>
            <th>Question 1</th>
            <th>Question 2</th>
            <th>Question 3</th>
            <th>Question 4</th>
            <th>Date of Response</th>
        </tr>
    </thead>
    <tbody id="JBody"></tbody>
</table>

<script>
  $(document).ready(function() {
    fetch('http://172.26.188.199:8086/api/sass/sassList', { mode: 'cors' })
    .then(response => {
      if (!response.ok) {
        throw new Error('API response failed');
      }
      return response.json();
    })
    .then(data => {
      for (const row of data) {
        // BUG warning/resolution - DataTable requires row to be single append
        $('#JBody').append('<tr><td>' + 
            row.username + '</td><td>' + 
            row.question1 + '</td><td>' + 
            row.question2 + '</td><td>' +
            row.question3 + '</td><td>' + 
            row.question4 + '</td><td>' + 
            row.doquestion + '</td></tr>');
      }
      // BUG warning - Jupyter does not show Datatable controls, works on deployed GitHub pages
      $("#JTable").DataTable();
    })
    .catch(error => {
      console.error('Error:', error);
    });
  });
</script>