---
title: Data analysis - open source strikes back
date: 2025-07-16 17:00:00 -0300
categories: [Python, Dash]
tags: [fuel, visualization, dashboard, plotly-dash, pandas]     # TAG names should always be lowercase

description: Introduction to Dash
---
## Hello world! A brief overview

Welcome to the first post of my first blog. I'll seize this "blog" opportunity to record a bit of my journey here (a chance to test my documentation skills? =D).

This is my second attempt at using open source to create visualizations. After my first attempt, I got a job as a contractor for Ford Motor Company and kind of pushed my way into being useful by surrounding myself with datasets and visualizations.

### How it all began

Almost exactly 4 years ago I got [this Udemy Python *scrapy* course](https://www.udemy.com/course/web-scraping-in-python-using-scrapy-and-splash). Weeks later I created a visualization based on the data I got and then [pushed it to Github](https://github.com/azihell/properties-dashboard). It was far from becoming a real app, hosted on the web and updated daily, but it was a fun project. At the time, me and my girlfriend (now my wife) were dreaming of understanding the logic of real estate pricing and I came up with the idea of getting real data so we could ground ourselves when deciding when the time came.

### Experience at Ford Brasil

As expected of a multinational, there's virtually no end to resources or data. Also, there's plenty of opportunity to customize your own slice of it. That's how I developed datasets and dashboards using Qlik Sense, Google BigQuery and Alteryx to provide for the Electrified Vehicles Quality team based here in South America (as much as I would love to showcase what I created, I am bound not to due to data security reasons)

It was really interesting getting to query through terabytes of connected vehicles cloud data tables, join them and unearth information that was revealing for our Quality purposes. Mostly we were looking for DTCs (diagnostic trouble codes) or other measurements that were recorded in tables during drive cycles. Information that could provide insights on unwanted vehicle behavior. Making sense out of the data was an effort divided among many colleagues, and I would like to leave my appreciation for this time we had.

### [Current project](https://github.com/azihell/anp_viz)

After learning how to use this compact suite of tools, I decided to face a new challenge: try to build similar to what I did, but this time with open (source) resources.
Bit by bit [it is taking shape as a visualization](https://anp-dashboard.onrender.com/) which is an attempt to reproduce, with Python scripts, most of what I learned to use (SQL and Qlik Sense).

