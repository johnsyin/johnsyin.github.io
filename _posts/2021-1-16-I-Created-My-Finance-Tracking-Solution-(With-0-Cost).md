---
layout: post
title: I Created My Finance Tracking Solution (With 0 Cost)
---

Back in March 2020 (yes, during MCO 1.0), I have created my own finance tracking solution, the current main users are my girlfriend and myself.  

----
# My Inspiration
The idea came from 
1. My inspiration from work
2. The inability to track my overall finance status, my personal balance sheet and my net worth change overtime with the existing tracking app.
3. The inflexibility for the existing tracking app to query/create charts that you want from your data.

## 1) My inspiration from work
It's no surprise that tools like Google Sheets/Google Form are both online tools that allows you to get and store data without hosting anything (and without being tech savvy). But the main inspiration comes from noticing that the same series of tool for visualization by Google is also both free-to-use and online solution, not to mention their integration with Google Sheet is so easy.

## 2) The inability to track my overall finance status
This might be abit ninche, but for a person like me who likes to track and record things in my life, I would want to know my investment return over years, my net worth change more than just tracking expenses. That said, there is no single app that already work like this.

## 3) The inflexibility for the existing tracking app to query/create charts that you want from your data
This is more self-explanatory, for anyone that knows excel/data studio, the app I have created gave you the flexibility to query data or create charts.

----
# The Implementation

![overall architecture]({{ site.baseurl }}/images/2021-1-16-overall-architecture.png "overall architecture")

## A) The form
I have put all purpose in a form for easier navigation, in case I get deposited my salary or I spent some money or I bought/sold some stocks, I record them down from this one form.
- For salary, I need to record which bank and how much I have get it.
- For expenditures, I can record the type of expenditure and fill remarks optionally.
- For stocks, I need to record purchase price and number of units.
- For transferring money between assets, I have to record the amount and the involved amount.
![form]({{ site.baseurl }}/images/2021-1-16-form.png "form")
All of these are tied to the record created date, in case if I am filling a historical record, I can override the date from the form optionally too.

## B) The sheets
There are tons of formula that are computed here which I am not going to into details, but basically, there are 5 sheets that made this work
1. The "form" sheet
- This sheet connects to the form directly and have some columns to compute some essential information (e.g. gross value of stock, mask expenses only, mask incomes only, etc.)
![form archives]({{ site.baseurl }}/images/2021-1-16-form-archives.png "form archives")

2. The "balance" sheet
- This sheet calculates from the form sheet to get the balance value on each asset I have
- This sheet extract stock codes using regex
- This sheet convert any non-MYR currency to MYR for me (using rate from google finance)
- This sheet uses google finance to scrape the stock price of the stock I own automatically
![balance]({{ site.baseurl }}/images/2021-1-16-balance.png "balance")

3. The "historical asset" sheet
- This sheet record my daily summary everyday 
  - My net buy position
  - My net worth
  - My portfolio holding value
  - Dividend
  - My return
  - Index (KLCI, )
  - etc.
![historical asset]({{ site.baseurl }}/images/2021-1-16-historical-asset.png "historical asset")

4. The "scraper" sheet
- Scraper helps me to scrape all indices.
![scraper]({{ site.baseurl }}/images/2021-1-16-scraper.png "scraper")

5. The "history brokerage" sheet
- Since brokerage can be abit complex to calculate and each broker can implement at a different rate, I have created a dedicated sheet to compute them. It's also for my convenience to see how much I have paid the broker.
![historical brokerage]({{ site.baseurl }}/images/2021-1-16-historical-brokerage.png "historical brokerage")

## C) Google App Script
The script is scheduled to run for 2 purposes.
1. To automatically copy current balance sheet's sum into historical asset for today
2. To check and copy scrape price data if there is no error. In some cases, google finance failed to fetch the stock prices, therefore, we only want to copy when there is no error. Using this way, the dashboard will be more robust during market hour (although it might not reflect real-time data).

## D) The dashboard (data studio)
I have created a 2 pages dashboard to keep track on 2 things:
1. My assets
2. My expenditure vs savings  
  
I will just show a screenshot with mocked data for demo purpose:
![dashboard]({{ site.baseurl }}/images/2021-1-16-dashboard.png "dashboard")

----
There you have it! I've had lots of fun making it and it been really useful to myself and my girlfriend, there were also lots of pain dealing with sheets formula.  
  
# What's next?
I do hope that I am able to share this tool with you, however, the whole app is setup manually and pretty inconvenient to replicate for another user. Recently, I have been getting more exposure on stuff like Infra-as-a-code for dev-ops to replicate environment robustly and quickly, I will propably explore more on the possibility on creating a pipeline that can automatically authenticate and create essential resource for the app for one google account in the future.
