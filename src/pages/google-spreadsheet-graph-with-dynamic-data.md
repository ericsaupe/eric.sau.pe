---
templateKey: blog-post
title: Google Spreadsheet Graph with Dynamic Data
date: '2014-03-27T10:31:00-07:00'
tags:
  - change graph when row is added
  - dynamic chart
  - dynamic graph
  - google
  - google apps
  - google apps scripts
  - google apps scripts spreadsheets
  - google script chart
  - google script dynamic graph
  - google script graph
  - google scripts
---
Recently we started dumping some data from the database into a shared spreadsheet on Google Docs.  With all of this data it would be helpful to visualize it in some way.  After searching the net it seems there isn't really a good way to create a chart that is dynamic with what data is in the spreadsheet.  What I want is to have my graph update every time a new row is added to the sheet without having to edit the graph directly.  The solution was to write a custom Google Apps Script to do just that.

To add a Google Apps Script to a document open the document and click Tools -&gt; Script editor...  This opens up a page for editing scripts.  If a prompt appears for what type of script you want to do just select Blank.  Now you just need to paste in this code below.
<pre class="lang:default decode:true">//Runs on document open
function onOpen() {
  createGraph();
};

//Runs on any edit
function onEdit() {
  createGraph();
}

/**
 * This function reads the data in the Totals columns
 */
function createGraph() {
  var tabName = 'My Sheet';
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName(tabName); // This grabs the tab with the name stored in tabName.
    // Read variables
    var startRow = 2;  // What row to start reading at
    var startColumn = 1;  // What column to start reading
    var numOfColumns = 3;  // Number of columns to read

    // Graph variables
    var posX = 5;  // Column to anchor graph to
    var posY = 2;  // Row to anchor graph to
    var width = 600; // Graph Width
    var xTitle = 'Week'; // XAxis Title
    var yTitle = 'Quantity'; // YAxis Title
    var chartType = Charts.ChartType.COLUMN; // Chart Type

    var range = sheet.getRange(startRow, startColumn, sheet.getLastRow(), numOfColumns);

    //If charts already exist, update them
    if (sheet.getCharts().length &gt; 0){
      var chart = sheet.getCharts()[0];
      chart = chart.modify()
      .removeRange(chart.getRanges()[0])
      .addRange(range)
      .setPosition(posY, posX, 0, 0)
      .setOption('width', width)
      .setOption('vAxis.title', yTitle)
      .setOption('hAxis.title', xTitle)
      .setChartType(chartType)
      .build();
      sheet.updateChart(chart);
    }
    //If not, create them
    else {
      var chartBuilder = sheet.newChart();
      chartBuilder.addRange(range)
      .setPosition(posY, posX, 0, 0)
      .setOption('title', 'Total Report Graph')
      .setOption('width', width)
      .setOption('vAxis.title', yTitle)
      .setOption('hAxis.title', xTitle)
      .setChartType(chartType);
      sheet.insertChart(chartBuilder.build());
    }
  }
  return true;
}</pre>
To make this work for you just change the tabName variable to whatever your tab is that you want to use the graph in, change the start row and start column of where your data begins and what column to end reading.  You don't need to specify an endRow because we will just be reading all of the data until there is no more.  After that you can customize where the graph will be located, how wide it is, and the titles of the axis.

You'll see that this method will get called anytime the document is edited or opened by someone who can edit.  Viewers who cannot edit will not trigger the event even on open.  That's all there is to it.  dynamic Google spreadsheet graphs are now just a click away and once you have it set up you'll never have to touch it again.

If you have any other Google Apps Scripts tips leave them below in the comments.
