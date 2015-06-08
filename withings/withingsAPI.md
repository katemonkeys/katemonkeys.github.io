#The top six things you need to know about the Withings API*

#####*where “you” is probably a developer, or at least a strange user

I should preface this by saying that I got a Withings Smart Body Analyzer for Christmas last year and I’ve been generally happy with it. It purports to be able to take my heart rate through my bare feet and that seems not to work for my physiology, but overall I’m a fan. If if their [Wikipedia page](http://en.wikipedia.org/wiki/Withings) is to be believed they are having a pretty rad impact on making the Quantified Self movement more for normal people and they only have 20 full time employees. Also they try hard to use SI units, which I can get behind. Anyway, on to the rant. 

I originally called this post “Everything wrong with the [Withings API](http://oauth.withings.com/api/doc)” and I meant it. For every useful field I can extract from their “award winning” app, I have spent an hour screaming at the inconsistencies in their implementation or inexplicable holes in their data coverage. This is as much a post for developers looking for reasons why they are tearing their hair out trying to access an unruly metric as it is for anyone who provides a third party API that is intended to be consumed by anyone not themselves, and what us poor data consumers have to deal with. 

It’s inconsistent. Sometimes we query for startdate (which needs to be formatted as a time in epoch seconds, for sleep measures) and sometimes for startdateymd (which returns a starttime in epoch milliseconds for sleep summaries). Some measures return device identifiers: a model 16, for example, is their Pulse watch, and a model 32 is their Aura bedmat. What then am I to make of a sleep data object reporting a model 61?? 

(Figure1-model61)

2. They don’t understand OAuth. “The Withings OAuth implementation is not perfect” says the gentle author in the README for the [withings-request node module](https://www.npmjs.com/package/withings-request). The author has helpfully rendered transparent a lot of the failings (encrypting the wrong url parameters, returning unparseable nonsense) to consumers of the data, and for this I am grateful. I’m not sure if this is a problem of multiple authors or what. Hey, try a [code review](http://eugenedvorkin.com/engineering-culture-and-why-it-is-matter-for-business).

(Figure2-picture of cat)

3. Not only does their API lack certain measures, like the Smart Body Analyzer is supposed to track air quality and it doesn’t show up even in v2 of the API. It’s their prerogative, I suppose, and might plausibly have the benefit of forcing people to come back to the mothership app? Anyway, even with an enabled subscription it will arbitrarily and for no reason not deliver days worth of data of one measure or another. When it does deliver the data it disagrees with the primary presentation: see below for a whole week without “level 3” (REM) sleep reported through the API, and with plenty of data points on the app. 

(Figure3a-appscreenshot)
Caption3b: This is the JSON output of the sleep *detail* for this past week. Notice not a single “state:3”, which would represent REM sleep
```javascript
{
model: 16,
series: [
{
startdate: 1421586709,
state: 2,
enddate: 1421587669
},
{
startdate: 1421587669,
state: 1,
enddate: 1421589589
},
{
startdate: 1421589589,
state: 2,
enddate: 1421593849
},
{
startdate: 1421593849,
state: 1,
enddate: 1421595229
},
{
startdate: 1421595229,
state: 2,
enddate: 1421596429
},
{
startdate: 1421596429,
state: 1,
enddate: 1421599309
},
{
startdate: 1421599309,
state: 2,
enddate: 1421601769
},
{
startdate: 1421601769,
state: 1,
enddate: 1421602189
},
{
startdate: 1421602189,
state: 0,
enddate: 1421603089
},
{
startdate: 1421603089,
state: 1,
enddate: 1421603569
},
{
startdate: 1421603569,
state: 0,
enddate: 1421603629
}
]
}

```
Caption 3c: The JSON output of the sleep *summary* data for this past week, indicating upwards of 5 minutes of REM sleep on a given night instead of multiple hours as in the app screenshot.

4. Conversely but just as irritatingly, for some days a user on a single account will get several sets of sleep summary data, with several mutually incompatible descriptions of the same night. 

(Figure4-contradictorynights)

5. Account linking and device sharing is *terrible*. My partner and I share a scale and prior to its purchase we had separate accounts, but couldn’t figure out how to get it to recognize a measurement as his without making him a new account which was subordinate to mine, which screwed up activity tracking through his own phone. Now he basically doesn’t use the step counter, so, fail. 

6. I wrote to them complaining about this and got a rapid, unhelpful response, where the link to the referenced press release 404s. Oh well. 

(Figure6-404)


In conclusion, Withings, you’ve got good products and I wish you the best as a company, but I quietly curse you about 30 times a day. Please don’t take it personally. 
