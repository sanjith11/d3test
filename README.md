<html>

<head>


</head>
<style type="text/css">

    .bar {
        fill: steelblue;
    }

    .bar:hover {
        fill: brown;
    }

    .axis {
        font: 10px sans-serif;
    }

    .axis path,
    .axis line {
        fill: none;
        stroke: #000;
        shape-rendering: crispEdges;
    }

    .x.axis path {
        display: none;
    }

</style>

<script src="http://d3js.org/d3.v3.min.js"></script>

<body>
<div id="chart"></div>

<script>
    var charData = [100, 220, 330, 440, 110, 230, 180, 100, 220, 330,
        440, 110, 230, 180, 500, 100, 220, 330, 440, 110,
        230, 180, 100, 220, 330, 440, 110, 230, 180, 500,
        100, 220, 330, 440, 110, 230, 180, 100, 220, 330,
        440, 110, 230, 180, 500, 100, 220, 330, 440, 110,
        230, 180, 100, 220, 330, 440, 110, 230, 180, 222,
        100, 220, 330, 440, 110, 230, 180, 100, 220, 330,
        440, 110, 230, 180, 500, 100, 220, 330, 440, 110,
        230, 180, 100, 220, 330, 440, 110, 230, 180, 500,
        100, 220, 330, 440, 110, 230, 180, 100, 220, 330];

    var barWidth = 10;
    var barOffset = 5;


    var margin = {top: 20, right: 20, bottom: 30, left: 40},
            width = 960 - margin.left - margin.right,
            height = 500 - margin.top - margin.bottom;

    var yScale = d3.scale.linear()
            .domain([0, d3.max(charData)])
            .range([0, height]);

    var xScale = d3.scale.ordinal()
            .domain(d3.range(0, charData.length))
            .rangeBands([0, width], .2);

    var xScaleForAxis = d3.scale.linear()
            .domain([0, d3.max(charData)])
            .range([0, width]);


    var xAxis = d3.svg.axis()
            .scale(xScaleForAxis)
            .orient("bottom")
            .ticks(0, "");

    var yAxis = d3.svg.axis()
            .scale(yScale)
            .orient("left")
            .ticks(0, "");

    var svg = d3.select("body").append("svg")
            .attr("width", width + margin.left + margin.right)
            .style("background", "white")
            .attr("height", height + margin.top + margin.bottom)
            .append("g")
            .attr("transform", "translate(" + margin.left + "," + margin.top + ")");


    svg.append("g")
            .attr("class", "x axis")
            .attr("transform", "translate(0," + height + ")")
            .call(xAxis)
            .append("text")
            .attr("x", width)
            .attr("dy", "1.5em")
            .style("text-anchor", "end")
            .text("Time");


    svg.append("g")
            .attr("class", "y axis")
            .call(yAxis)
            .append("text")
            .attr("transform", "rotate(-90)")
            .attr("y", 6)
            .attr("dy", ".71em")
            .style("text-anchor", "end")
            .text("Message Count");

    var chart = svg.selectAll("rect").data(charData)
            .enter().append('rect')
            .attr("class", "bar")
            .attr('width', xScale.rangeBand())
            .attr('height', function (d) {
                return yScale(d);
            })
            .attr('x', function (d, i) {
                return xScale(i);
            })
            .attr('y', function (d) {
                return (height - yScale(d));
            })

    var toolTip = d3.select('body').append('div')
            .style('position', 'absolute')
            .style('background', '#ccc')
            .style('opacity', '0')
            .style('padding', '5 15px')
            .style('border', '1px #000 solid')
            .style('border-radius', '10px');


    chart.on('mouseover', function (d) {

        toolTip.html(d)
                .style('left', (d3.event.pageX) + 'px')
                .style('top', (d3.event.pageY) + 'px')

        toolTip.transition()
                .style('opacity', '1.0');
    })

    chart.on('mouseout', function (d) {
        toolTip.transition()
                .style('opacity', '0.0');
    })


</script>


</body>

</html>
