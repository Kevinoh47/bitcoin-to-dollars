Bitcoin to Dollars API

Below is an API endpoint that will spit out the last 100 entries of bitcoin to USD.
https://www.bitmex.com/api/v1/instrument/compositeIndex?symbol=.XBT&filter=%7B%22timestamp.time%22%3A%2210%3A55%3A00%22%2C%22reference%22%3A%22BSTP%22%7D&count=100&reverse=true
From that, output JSON to a file, endpoint, or browser  with these values:
[
    {
        “date”:"{date}”,
        “price”:”{value}”,
        "priceChange": "{up/down/same}",
        "change": "{amount}",
        "dayOfWeek":"{name}”,
        "highSinceStart": "{true/false}”,
        “lowSinceStart": "{true/false}”
    }
]
Results ordered by oldest date first:
 
- "Price change" is since previous day in the list, first day can be “na”
- "change" is the difference between previous day in list. “na” for first
- "day of week" is name of the day (Saturday, Tuesday, etc)
- "high since start” / “low since start” is if this is the highest/lowest price since the oldest date in the list.
Example:
Day1 :
  Value:100
  Highest:true
  Lowest:true
Day2:
  Value:90
  Highest:false
  Lowest:true
Day3:
  Value:101
  Highest:true
  Lowest:false
Etc…
 
- It should be written in node or python, and be hosted on GitHub.
 