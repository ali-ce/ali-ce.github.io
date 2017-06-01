---
layout: post
title: "How I made the Oscar database"
date: 2014-03-01 18:24:44 +0200
author: "Alice"
categories:   tutorial
tags:         
excerpt_separator: <!--more-->
---
This post will show you how I created the Oscar Database Silk ([oscars.silk.co](http://oscars.silk.co/)), so you can create something like it yourself and dig into your own stories and visualizations.   
<!--more-->  
The [Oscar Database Silk](http://oscars.silk.co/) merges the information on the winners and nominees of all the 86 Academy Awards with:

1.  Data from [IMDBb](www.imdb.com) on users’ rating, genres, countries and directors.
2.  [Andy Baio’s](https://waxy.org/) analysis of [pirated copies of Oscar screeners](https://docs.google.com/spreadsheets/d/1H8eds6jEe-BXoXIFH1RdbgigVtWdpyc8A9gyqHUt4Do/pub?hl=en_US&hl=en_US&output=html)
3.  Statistical evidence of [the gender gap at the Academy Awards](https://oscars.silk.co/page/Gender%20Gap%20at%20the%20Academy%20Awards)


<h2>Data collection for the Oscar Database</h2>

[Wikipedia](www.wikipedia.org), as well as many other sources, already has data on the nominees and winners for each Academy Awards Ceremony. But this is nothing new: every year we see the usual statistics on who returns home with more statuettes, and who disappoints expectations. So we thought we’d make this basic data come alive, by enriching it with facts pulled from across the web, and importing everything in a Silk to make it interactive. How we managed? Here’s a simple step-by-step guide, so that you can repeat our experiment.

<h3>Aggregating data from three sources.</h3>

To keep it easy, we did everything resorting almost exclusively to Google Sheets. Here is a screenshot of how we structured the data, so that it would be easy to visualize in Silk, where each movie must have a unique ID: 

<image src="http://www.alice-corona.nl/portfolio/wp-content/uploads/2016/03/tumblr_inline_n1r8htZSo91sajt1m-300x156.png">   

**1.[OMDb](www.omdbapi.com): The Open Movie Database API by Brian Fritz**   
We used these APIs to pull in data from [IMDb](www.imdb.com). We added a column in the Google spreadsheet where we generated the url to look up each of the about 4500 Oscar nominated movies. This is what the result looks like, and below is a step-by-step guide on how to achieve it.  

<image src="http://33.media.tumblr.com/95aaba51833b0da64b1ca98dc7271cb6/tumblr_inline_n1r8i4eQOi1sajt1m.png" style="max-width: 80%;">

- Generate a column where you convert the title of the movies to lowercase and replace white spaces with “+”. You can do this easily with the formula LOWER and then using “find and replace” to transform whitespaces into “+”. This is the third column you see in the previous screenshot. Be careful also to delete any special characters that can’t appear in urls.
- An example url for the API request is

`[http://www.omdbapi.com/?t=true+grit&y=1969](http://t.umblr.com/redirect?z=http%3A%2F%2Fwww.omdbapi.com%2F%3Ft%3Dtrue+grit%26y%3D1969&t=MmRhYWZkZjNhYTMwNjc2ZDc2NjM1Y2U0YjUwMmEwOTA1ZDkzYWVhMCxQN0JQd0RDYw%3D%3D)`  

So we wanted to create a column where each movie had its own url. To do this, in cell D2, type (and then drag down for the whole column): `="http://www.omdbapi.com/?t="&C2&"&y="&B2`

- Once we have the urls for each movie, we have to get the actual data from these urls. You can research how to use xml and XPath for scraping…or you can just type this easy formula in cell E2 and then drag it through the whole column:

`=IMPORTXML(D2,"/html/body/text()")`

- Now all you have to do is clean up the text data that is in column E and split it into the columns you desire. You can do this with the SPLIT spreadsheet formula, or with your favorite text wrangler. Personally, I prefer [Open Refine](http://openrefine.org), which also allows me to double check the data for consistency.

You’ve just assembled your Academy Awards & IMDb dataset, congratulations! Now you can stop here, import it in [Silk](www.silk.co) and start browsing it or visualizing it. Otherwise, read on to discover how to enrich it even more.

**2. Free more data with [Freebase](http://www.freebase.com)**

[Open Refine](www.openrefine.org), that I’ve mentioned before, has an awesome extension that allows you to combine your spreadsheet with data from [Freebase](www.freebase.com), *“a community-curated database of well-known people, places, and things”*. In this case, we worked with a simplified version of the dataset, where each individual nominee has its own row. The spreadsheet looks like this: 

<image src="http://www.alice-corona.nl/portfolio/wp-content/uploads/2016/03/tumblr_inline_n1r8e7jTtc1sajt1m-300x131.png" style="max-width: 80%;">  

Import your Oscar data in Open Refine, select the column where you have the names of the candidates and click on “Reconcile”. Here a pop-up window will guide you through the process. Once the column is reconciled, select it again and this time click `“Edit column —> Add columns from Freebase”`.
You can now add as many columns as you like, with information about the candidates. I added only gender, but you can also see the year of birth, or the nationality, or what movies it starred in… So don’t stop your imagination!

**3. Andy Baio’s piracy data**

After you’ve tackled the worst - XPath and Freebase - this last step will seem a piece of cake. [Andy Baio’s](www.waxy.org) already did all the dirty data collection work, and has a beautifully clean csv he shares at the end of [his blogpost](http://waxy.org/2012/01/mpaa_wins_the_oscar_screener_battle_but_loses_the_war) on Oscar and piracy. All you’ve got to do is merge his csv, which covers Oscars from 2002, with yours. You can do this in many ways (Excel, Fusion Tables..). To keep it simple, let’s use the VLOOKUP formula in Google Spreadsheet. Look at the following screenshot for reference: 

<image src="http://33.media.tumblr.com/2188baebf0daaf9b23af2401e2154980/tumblr_inline_n1r8frrUMq1sajt1m.png" style="max-width: 80%;">!

*   Import Andy’s file in a new sheet of the spreadsheet your working on. Add a column named “ID” where you join the movie with the year in brakets, so that it matches the column ID of your original dataset.
*   In the sheet with your original data: add as many columns as the columns you want to take from Andy’s data, and filter your original data for movies from 2002 on. In this case we added two columns, one for ‘Screener Release to Screener Leak’ and one with the Yes/No answer to whether the screener had leaked before Oscar night.
*   Start merging the two data sources with VLOOKUP. In the cell corresponding to the first 2002 movie (in my case row 3989) that will contain information on how many days passed between a screener release and its leaking online (in my case cell AZ3989), type:`=VLOOKUP(A3989,AndyPiracyData!A:P,14,false)`

The number “14” refers to the column ‘Screener Release to Screener Leak’ of the sheet with Andy’s data. Drag this formula down till the end of the sheet. Move to the next column, where we’ve decided to place the information on whether a screener had leaked by Oscar night. Type the same formula as before, only changing the ‘14’ to ‘16’. Again, drag it down. This should have gotten most of the data. There are movies for which the formula will return “#N/A”. This could be for two reasons: 

1. Andy has collected data only for full-length feature films, excluding documentary and foreign films. So you might have more movies than he has. 
2. The movies are spelled differently between the two datasets. Solve this by double checking and manually copy-pasting the information. Don’t worry, it’s only a couple of instances that have this problem!

<h2>If you’ve survived the tutorial..get ready to play!</h2>

You liked this blog post? Then use Silk for your own data journalism project: We’ve guided you through the steps we went through to create this Oscar data Silk, and you can adapt them to any other data source of your choice. We’ve covered the Oscars, but don’t you think that the Sundance Festival, the BAFTA, or the Golden Globe deserve the same treatment and will reveal more stories? And how about your favorite music festival? Free the data and [create a Silk](www.silk.co)!

*Originally published on [Silk's blog](http://blog.silk.co/post/78199230161/how-we-made-the-oscar-database)*