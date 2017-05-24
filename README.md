# tma2017-ripeatlas-exercise

## Abstract 

Ripe Atlas is a massively distributed Internet measurement platform that allows for a multitude of pre-defined measurements. In this lab, we will take a research question posed by one of the TMA main conference publications. Step by step, we will discover which features of Ripe Atlas can be used to answer a complex research question, and which subtleties should be taken into consideration when formulating your measurement strategy.
We will walk through scheduling measurements, obtaining, and parsing results, to how to integrate parsed results into your analyses scripts. The specific measurements covered will be distrubted DNS lookups followed by distributed path measurements, representing a typical measurement pipeline in a research paper.

## Details

### Introduction: Research Questions and overall measurement strategy (10 Minutes)

RQ: How many networks do you need eavesdropping capabilities to surveil a majority of APNs backend logins?

Steps to be taken:

1. What are the backend servers?
2. How to measure user population connecting to backend servers?
3. How to map this to networks? What is a network in this context?
	
### Step 1: Finding the backend servers (20 minutes)

From passive observations, we know that clients resolve [1-50]-courier.push.apple.com and then (at 1 passive vantage point) connect to one IP address in the 17/8 range.

How to find all global APNs backend IP addresses? --> DNS lookup using Ripe Atlas

Details: How many probes per lookup? Which detailed setting? How to run the measurement and obtain the result?

`Emile, do we want ~30 people to run the lookups? Or should each person eg lookup 3 DNS names?`

`Note: Geographical Clustering -- would be better to precisely map IP addresses to geography`

Once results are obtained, they need to be parsed into target IP addresses. `TODO: provide our script here`

step1: 50 DNS names -> resolve to IPs with RIPE Atlas probe resolvers
   -> TODO: help with abuf (stephane bortzmeyer tools?)

### Step 2: Path Measurements using Traceroute (30)

Now we have a set of IP addresses. How to schedule our traceroute measurements? What are the parameters, what is our strategy?

1. Should we measure towards all the IP addresses? Or some? Which? Why?
	* What do you want to learn from the measurements? 
2. What probes should we measure from? All? Some? How to select them? Why do they/do they not represent the APNs user base?
	* Note: In the paper, we just selected 1000 random probes per IP address. A better strategy would have been to only pick probes that were directed towards a certain IP address, but this is significantly more effort. Also, controling for country and source AS might be an idea (but getting 1000 random probes maybe better resembles user population than picking 1 probe per AS)

`TODO: Everyone run the measurements for her/his country (only pick DNS replies from that country) OR everyone run global? do range-partitioning on IP addresses to balance load among participants?`

### Step 3: Analyze results (20 minutes)

* How to download the results?
* How to parse them? Discuss raw json vs. eg cousteau 
* How to map to "network", i.e. AS or IXP?

Ideally, everyone runs my script to get the results for his/her country

`TODO flesh this out?`

### Step 4: Discuss usefulness, compare results


# More ideas, old notes

`Ideally, this exercise would also result in pull requests to refine the reproducibility documentation`


step2: you cannot trace towards all these IPs
   -> what strategy to use to reduce, where you can argue you won't loose measurement accuracy
   -> thought exercise -> not coding per-se
   
step3: traceroutes from probes towards the target IPs   goal: use APIs or UI, discover parameters
   -> probe selection?  easiest: ask atlas for 1000 probes
      -> one probe per AS + country? other selection strategies?
   -> traceroute parameters -> discussion
   -> split up the list of IPs and give groups different parts of that list

(measurements take 10 mins, some discussion here. TODO)

step4: analysing the traceroutes.  goal: use API to download results, parse ripe atlas data (raw json, or use cousteau)
   -> how many source ASNs and how many countries in the results?
   -> TODO anything simple to extract from the traceroutes
   -> just discuss the concepts?

end result: run quirin's script and compare to results in the paper?
pick your own country and compare to the Germany results in the paper?
 - are they different, and can you think of reasons why different or not?
