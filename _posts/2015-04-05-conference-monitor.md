---
layout: post
title: Conference Monitor
comments: True
---

Recently I started experimenting with [Highcharts](http://www.highcharts.com)
to visualize [our conference metrics](http://bvasiles.github.io/papers/scico13.pdf) 
in the browser. Here's a sample. Conference organizers may find these useful.
Click (tap) on conference names in the legend below each plot to enable those 
series. The data is stored in [this GitHub repo](https://github.com/tue-mdse/conferenceMetrics) 
and loaded dynamically on page load.

### Acceptance rates

##### Submissions
Number of papers submitted to the *main research track*.
**Update:** For MSR this is the number of *full* papers submitted to the 
main research track. Thanks to [Tom Zimmermann](http://thomas-zimmermann.com)
for the clarification.

<div id="container_sp" style="min-width:310px; height:400px; margin:0 auto; width:800px;"></div>

<!-- (without distinguishing between long and short papers, if both were part of the main track). -->

##### Acceptance rates 
Ratio between the number of accepted full papers and the number of submissions.

<div id="container_ra" style="min-width:310px; height:400px; margin:0 auto; width:800px;"></div>

### Program committees

##### PC size 
Number of PC members.

<div id="container_c" style="min-width:310px; height:400px; margin:0 auto; width:800px;"></div>

##### Review load 
Ratio between number of submissions and number of PC members.
If each submission is reviewed by 3 PC members, multiply each value by 3 to
see how many submissions are assigned to each PC member.

<div id="container_rl" style="min-width:310px; height:400px; margin:0 auto; width:800px;"></div>

### PC turnover

Fraction of PC members in a given year which are new with respect to the 
previous year.

<div id="container_rnc1" style="min-width:310px; height:400px; margin:0 auto; width:800px;"></div>

### Openness

##### Papers by new authors (w.r.t. the previous 4 editions)
Fraction of accepted papers that have been co-authored by *new authors*, i.e., 
papers for which none of the authors published at any of the previous 4 editions 
of this conference.

<div id="container_rpna4" style="min-width:310px; height:400px; margin:0 auto; width:800px;"></div>

##### Papers by PC members
Fraction of accepted papers that have been co-authored by at least one PC member.

<div id="container_rac0" style="min-width:310px; height:400px; margin:0 auto; width:800px;"></div>

#### Note

You can contribute additional data by submitting pull requests to 
[this repository](https://github.com/tue-mdse/conferenceMetrics).

More details about the metrics:
> [How healthy are software engineering conferences?](http://bvasiles.github.io/papers/scico13.pdf)
> *Vasilescu, B., Serebrenik, A., Mens, T., Brand, M.G.J. van den, and Pek, E.*
> Science of Computer Programming 89, Part C, (2014), 251–272.



<script src="http://code.highcharts.com/highcharts.js"></script>
<script src="http://code.highcharts.com/modules/data.js"></script>
<script src="http://code.highcharts.com/modules/exporting.js"></script>

<script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/jquery/1.8.2/jquery.min.js"></script>

<!-- Additional files for the Highslide popup effect -->
<script type="text/javascript" src="http://www.highcharts.com/media/com_demo/highslide-full.min.js"></script>
<script type="text/javascript" src="http://www.highcharts.com/media/com_demo/highslide.config.js" charset="utf-8"></script>
<link rel="stylesheet" type="text/css" href="http://www.highcharts.com/media/com_demo/highslide.css" />

<script type="text/javascript">
$(function () {

	function myChart(myTitle, myYLbl, csvPath, container) {
		var options = {
			chart: {
				renderTo: container,
				type: 'line'
			},
			title: {
				text: myTitle
			},
			xAxis: {
				categories: []
			},
			yAxis: {
				title: {
					text: myYLbl
				}
			},
			series: [],        
			tooltip: {
				formatter: function() {
				  var s = [];
				  $.each(this.points, function(i, point) {
					s.push('<span class="tooltip">' + point.series.name + ' : ' +
					  point.y + '<br><span>');
				  });
				  return s.join('');
				},
				shared: true
			}
		};

		// Get the CSV and create the chart
		$.get(csvPath, function(csv) {
			var dataColumns = [];
			var dataNames = [];
			var xLabels = [];
		
			// Split the lines
			var lines = csv.split('\n');

			// Extract categories from header
			// year,ICSE,ICSM,WCRE,CSMR,MSR,GPCE,FASE,ICPC,FSE,SCAM,ASE
			var header = lines[0];
			var items = header.split(';');
			for (i=1;i<items.length;i++) {
				dataColumns.push([]);
			}
			dataNames = items.slice(1,items.length);

			// Extract data for the most recent 10 years        
			// 2013,50,71,65,64,46,27,30,50,37,36,33
			// 2012,48,70,65,64,41,32,31,38,36,42,44
			var rlines = lines.slice(1, 11);
			rlines.reverse();

			$.each(rlines, function (lineNo, line) {
				var items = line.split(';');

				$.each(items, function (itemNo, item) {
					// Data is from column 1 onwards
					if (itemNo > 0) {
						if (item.length == 0){
							var val = null;
						}else{
							var val = parseFloat(item);
						}
						dataColumns[itemNo-1].push(val);
					}
					// The year is column 0
					else{
						xLabels.push(item);
						options.xAxis.categories.push(item);
					}                    
				});
			});

			// Create data series                
			for (i=0;i<dataColumns.length;i++) {
				var r = dataColumns[i];
				var series = {
					data: r,
					name: dataNames[i]
				};
				if (series.name !== "ICSE" & 
						series.name !== "FSE" &
						series.name !== "ICSM") { 
					series.visible = false; 
				}
	
				options.series.push(series);
			}
			var chart = new Highcharts.Chart(options);
		});
	}

	myChart('Submissions', 
			'#Submitted_papers', 
			'https://gist.githubusercontent.com/bvasiles/25b581b408928c547e21/raw/SP.csv',
			'container_sp');
	
	myChart('Acceptance Rates', 
			'#Accepted_papers / #Submitted_papers', 
			'https://gist.githubusercontent.com/bvasiles/25b581b408928c547e21/raw/RA.csv',
			'container_ra');

	myChart('Program Committee Size', 
			'#PC members', 
			'https://gist.githubusercontent.com/bvasiles/25b581b408928c547e21/raw/C.csv',
			'container_c');
			
	myChart('PC Turnover', 
			'Fraction PC new w.r.t. previous year', 
			'https://gist.githubusercontent.com/bvasiles/25b581b408928c547e21/raw/RNC1.csv',
			'container_rnc1');
			
	myChart('Review Load (x 3)', 
			'#Submissions / #PC_members',
			'https://gist.githubusercontent.com/bvasiles/25b581b408928c547e21/raw/RL.csv',
			'container_rl');
			
	myChart('Papers by New Authors (w.r.t. previous 4 years)', 
			'Fraction papers by new authors', 
			'https://gist.githubusercontent.com/bvasiles/25b581b408928c547e21/raw/RPNA4.csv',
			'container_rpna4');

	myChart('Fraction Papers by PC members', 
			'Fraction PC papers',
			'https://gist.githubusercontent.com/bvasiles/25b581b408928c547e21/raw/RAC0.csv',
			'container_rac0');
});
</script>

