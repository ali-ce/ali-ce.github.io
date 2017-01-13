---
layout: post
title: "Data journalism: extracting and scraping data"
date: 2016-04-13 18:24:44 +0200
author: "Alice"
categories:   tutorial
tags:         
excerpt_separator: <!--more-->
---
This post on extracting data from a website is the first in a 3-part series on extracting, cleaning and enhancing data.  
<!--more-->  

Some sites already have their data in a neat table, allowing you to easily copy and paste it into a spreadsheet. Wikipedia is a good example of this. Others offer their data for download in a spreadsheet format. However, it’s not always that easy. If you come across a website with structured information that isn’t downloadable or organized in a table, you might have to turn to specialized tools to extract your data. Here are some of the ones we use at [Silk.co](https://www.silk.co/) to create our own data stories. And the best part? You don’t need to know how to code for using the tools listed below, just a bit of patience and a good idea of what you’re looking for. Good luck!

<h2>Import.io</h2>
[Import.io](https://www.import.io/) is an amazing tool that lets you extract data from any relatively structured website. Enter a URL on their homepage, and you will be quickly greeted by a structured data table. You can then export the data to a spreadsheet for further cleaning and enhancing.

<figure style="align:center;"><img style="border: 1px solid #eee;" src="https://silk-blog.s3.amazonaws.com/blogpost-images/magick.png" alt="" /></figure>

In the sweet case that no clean up is necessary and you want to build a Silk, you can even skip that last step. Silk actually has a [built in data extractor powered by Import.io](http://blog.silk.co/post/133509579947/silk-importio-instantly-transform-web-pages). Just [sign up for a Silk account](https://www.silk.co/signup), hit ‘Extract data from a website’ and enter a URL. Import.io then extracts data from the URL, and Silk turns it into a fully searchable database, with the option to create visualizations and data stories.

<h2>Your Browser’s Developer Tools</h2>
Sometimes, when you can’t copy data over from the web page itself, you can actually copy it from the HTML source. The HTML source is available from your favourite browser’s developer tools. The ‘Source’ tab shows you the page’s source. This can help you monitor which scripts pull the data and from where. Once you have this information, you can be lucky enough to actually find the URL that directs you to a clean structured data. Still following? Here’s an example. This [interactive visualization](http://www.pbs.org/wgbh/pages/frontline/concussion-watch/#players_2014) on NFL concussion counts appears pretty hard to scrape.

<figure style="max-width: 797px;"><img style="border: 1px solid #eee;" src="https://silk-blog.s3.amazonaws.com/blogpost-images/datapostpart1-1.png" alt="" /></figure>

But if you click in ‘Sources’ and analyze the code, you’ll find this interesting piece of information.

<figure style="max-width: 805px;"><img style="border: 1px solid #eee;" src="https://silk-blog.s3.amazonaws.com/blogpost-images/datapostpart1-2.png" alt="" /></figure>

We end up with [this link](http://www.pbs.org/wgbh/pages/frontline/js/data/concussions/automated_newer.json), which contains a structured JSON file, ready to be converted to a spreadsheet file with a tool like [OpenRefine](http://openrefine.org/) (we’ll cover OpenRefine and other tools in our next post on cleaning data). Sometimes, parts of a web page, such as interactive maps, are populated with data retrieved through API calls. A good alternative to scrape this data is to capture this flow of information by clicking on the ‘Network’ tab. Here, you’ll be able to monitor the network operations executed by the script used to run the web page. Sorting for bigger sizes and specific types of operation usually helps finding the script which returns the data you need. Then, you copy and paste the results previewed in the ‘response’ tab on the right into a text editor, and end up with a file containing the results of the API call used to retrieve the data you wanted.  

<h2>The Google Chrome Scraper Extension</h2>
[Scraper is a Google Chrome extension](http://mnmldave.github.io/scraper/) that converts data from a webpage into a spreadsheet. After you install the extension, visit the URL you want to scrape. Highlight an instance of the text you would like to convert to a table, right-click and choose ‘Scrape similar…’. You can directly export the results to a [Google Sheet](https://www.google.com/sheets/about/), or tweak the [Xpath](http://www.w3schools.com/xsl/xpath_syntax.asp) values until you are satisfied with the result. [Here is a video tutorial](https://youtu.be/oCp9IcdSpZI).

<figure><img style="border: 1px solid #eee;" src="https://silk-blog.s3.amazonaws.com/blogpost-images/datapostpart1-3.png" alt=""></figure>  
<br>
  
<h2>Google Sheets' ImportXML Function</h2>
One of the most useful things when it comes to harvesting data from the web is learning how to use XPath expressions. XPath is “[a query language for selecting nodes from an XML](http://en.wikipedia.org/wiki/XPath)”. Knowing how to use XPath to parse web pages will allow endless scraping possibilities, and help you build a spreadsheet from scratch. You can read more [here](http://www.w3schools.com/xpath/xpath_nodes.asp) and [here](http://archive.oreilly.com/pub/a/perl/excerpts/system-admin-with-perl/ten-minute-xpath-utorial.html). For less complex scrapers, you’ll find that you don’t really need a deep knowledge of XPath to use it. For example, you can easily use XPath to construct web crawlers that turn thousands of pages into a structured spreadsheet. Let’s say I have a list of cities, and I want to pull in information about each from Wikipedia. This is how I would start:

<figure style="max-width: 582px;"><img style="border: 1px solid #eee;" src="https://silk-blog.s3.amazonaws.com/blogpost-images/datapostpart1-4.png" alt="" /></figure>

To fill the image column, in cell C2, type: `=importXML(B2,"//td/a/img/@src")` The drag it down for all the other cities. `//td/a/img/@src` is the XPath expression to query the url (stored in B1) and return the results contained in that specified path. To understand the exact path of what you need, you can view the source of a webpage and figure it out yourself. Or you can find the object you want (in this case the image), by right clicking on it and selecting ‘Inspect Element’. You will then view the page source. Find the url of the image you want, right click and select ‘Copy XPath’. You will now have saved on your clipboard the XPath needed to retrieve the image.
<br>
<figure><img style="border: 1px solid #eee; width: 60%;" src="https://silk-blog.s3.amazonaws.com/blogpost-images/datapostpart1-5.png" alt=""></figure>

You can repeat this to fill out all the other columns. For example, the first paragraph describing a city is accessible through: `=JOIN(" ",importXML(B2, "//div[4]/p[1]"))`  
<br>

<h2>To Conclude</h2>

Ideally, data will always be organized into neatly downloadable packages, but until that becomes the case, we hope this post helps you find the data you need.. If you don’t succeed after your first attempt, please don’t be afraid to try a few of the other options listed here. A bit of persistence goes a long way! If you liked this post, follow our [Twitter account](https://www.twitter.com/silkdotco) to get updates on the next installment of this post and more data-greatness.

*Originally published on [Silk’s blog](http://blog.silk.co/post/142737643047/data-journalism-tools-part-1-extracting-and).*