# tma2017-ripeatlas-exercise

Emile Aben, Quirin Scheitle

## Abstract 

Ripe Atlas is a massively distributed Internet measurement platform that allows for a multitude of pre-defined measurements. In this lab, we will take a research question posed by one of the TMA main conference publications. Step by step, we will discover which features of Ripe Atlas can be used to answer a complex research question, and which subtleties should be taken into consideration when formulating your measurement strategy.
We will walk through scheduling measurements, obtaining, and parsing results, to how to integrate parsed results into your analyses scripts. The specific measurements covered will be distrubted DNS lookups followed by distributed path measurements, representing a typical measurement pipeline in a research paper.

#### Learning Objectives

* How to use Ripe Atlas for various Internet measurement tasks
	* Using DNS and Traceroute measurements
	* Learning the toolchain to process these
* Learning the Limitations and Broader Issues with Internet Measurements Using Ripe Atlas
	* Internet User Population vs. Ripe Atlas population

## Details

### Introduction: Research Questions and overall measurement strategy (10m)

RQ: How many networks do you need eavesdropping capabilities to surveil a majority of APNs backend logins?

Steps to be taken:

1. What are the backend servers?
2. How to measure user population connecting to backend servers?
3. How to map this to networks? What is a network in this context?
	
### Step 1: Finding the backend servers (20 minutes)

From passive observations at 1 Vantage Point (VP), we know that clients resolve [1-50]-courier.push.apple.com and then connect to 1 IP address in the 17/8 range.

How to find all global APNs backend IP addresses? --> Globally distributed DNS lookups using Ripe Atlas

*Task1: Pick 1 of `[1-50]-courier.push.apple.com` and do a RIPE Atlas DNS resolution, focusing on your home country (5 minutes)*  
Example: 42-courier.push.apple.com

`TODO: We need a lot of Ripe Atlas accounts and some credits for this`
`@Emile: I suggest people are allowed to collaborate, is that fine?`

After Task1: Discuss -- How did they interpret "focus on country"? how many probes per lookup? Which probes? Which detailed setting? How to run the measurement and obtain the result?

[Sample measurement from paper](https://atlas.ripe.net/measurements/5500016/)  
[Sample measurement from June 2017](https://atlas.ripe.net/measurements/8831682)  
[Script for batch measurements](https://github.com/tumi8/cca-privacy/blob/master/ripe_atlas/dns/atlas-measure.sh)


*Task2: Now obtain the measurement result (from own, as a fallback pick 1 of the paper measurements), then parse it. Use [this script](https://github.com/tumi8/cca-privacy/blob/master/ripe_atlas/dns/parse-results.py) as a start.*

[Sample measurement result from paper](https://github.com/emileaben/tma2017-ripeatlas-exercise/blob/master/data/result-5500014.json)  
[Sample measurement result from June 2017](https://github.com/emileaben/tma2017-ripeatlas-exercise/blob/master/data/RIPE-Atlas-measurement-8831682.json)  
[Our parsing script](https://github.com/tumi8/cca-privacy/blob/master/ripe_atlas/dns/parse-results.py)  
[Sample parsed result from paper](https://github.com/emileaben/tma2017-ripeatlas-exercise/blob/master/data/result-5500014.json.parsed.txt)  
[Sample parsed result from June 2017](https://github.com/emileaben/tma2017-ripeatlas-exercise/blob/master/data/RIPE-Atlas-measurement-8831682.json.parsed.txt)  

After Task2: Discuss result -- what do we see in the parsed results? A "CNAME - CNAME - A" chain, depending on geography

(Open 1 of the parsed results above, github displays them nicely)

--> Reply depends on geography of probing node, keep this in mind!

Wrap-up discussion: Now we have found the backend servers for 1 country -- are these all servers? Might some be missing?

### Step 2: Path Measurements using Traceroute (30 min)

Now we know a list of backend servers. 

*Task1: Define a measurement strategy: What RIPE Atlas settings and probes would you use? Which targets would you probe? Please only probe 1 IP address, though.*

*After Task1* Discuss:

1. Should we measure towards all the IP addresses? Or some? Which? Why?
	* What do you want to learn from the measurements? 
2. What probes should we measure from? All? Some? How to select them? Why do they/do they not represent the APNs user base?
	* Note: In the paper, we just selected 1000 random probes per IP address. A better strategy would have been to only pick probes that were directed towards a certain IP address, but this is significantly more effort. Also, controling for country and source AS might be an idea (but getting 1000 random probes maybe better resembles user population than picking 1 probe per AS)
3. What specific traceroute settings? TCP? Port?


Intermediate results:

[Traceroute measurement from Paper -- Germany -- Scripts](https://atlas.ripe.net/measurements/5719601/)  





### Step 3: Analyze results (20 minutes)

* How to download the results?
* How to parse them? Discuss raw json vs. eg cousteau 
* How to map to "network", i.e. AS or IXP?

Ideally, everyone runs my script to get the results for his/her country


Task1: Download traceroute result, parse it, think about evaluation strategy

After Task1 -- Discussion: What to do with the traceroutes? Come back to Research Question -- how many networks does an adversary have to eavesdrop on to see a majority of APNs logins? --> Map IP addresses in traceroutes to "networks". What is a network? How to do the mapping?

Task2: Map tracerouts to networks -- what does the result look like? Create a table similar to Table X in the paper.

After Task2: Discuss: Data Structure, Use all networks or exclude some (excluding the APNs AS is reasonable)

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
