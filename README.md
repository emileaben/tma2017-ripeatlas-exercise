# tma2017-ripeatlas-exercise

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

### Introduction: Research Questions and overall measurement strategy (10 Minutes)

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

Sample measurement from paper: https://atlas.ripe.net/measurements/5500016/
Sample measurement from June 2017: https://atlas.ripe.net/measurements/8831682



*Task2: Now obtain the measurement result (from own, as a fallback pick 1 of the paper measurements), then parse it (give code for abuf parsing).*

Our [parsing script](https://github.com/tumi8/cca-privacy/blob/master/ripe_atlas/dns/parse-results.py).

After Task2: Discuss result -- what do we see in the parsed results? A "CNAME - CNAME - A" chain, depending on geography

`Note: Geographical Clustering -- would be better to precisely map IP addresses to geography`
   
Link to [Intermediate Results and Parsing Scripts](https://github.com/tumi8/cca-privacy/tree/master/ripe_atlas/dns)

Example Parsed Intermediate Result for Discussion:

```
id 54717
opcode QUERY
rcode NOERROR
flags QR RD RA
;QUESTION
1-courier.push.apple.com. IN A
;ANSWER
1-courier.push.apple.com. 4636 IN CNAME 1.courier-push-apple.com.akadns.net.
1.courier-push-apple.com.akadns.net. 8 IN CNAME pop-eur-central-courier.push-apple.com.akadns.net.
pop-eur-central-courier.push-apple.com.akadns.net. 59 IN A 17.252.28.22
pop-eur-central-courier.push-apple.com.akadns.net. 59 IN A 17.252.28.18
pop-eur-central-courier.push-apple.com.akadns.net. 59 IN A 17.252.28.12
pop-eur-central-courier.push-apple.com.akadns.net. 59 IN A 17.252.28.30
pop-eur-central-courier.push-apple.com.akadns.net. 59 IN A 17.252.28.15
pop-eur-central-courier.push-apple.com.akadns.net. 59 IN A 17.252.28.32
pop-eur-central-courier.push-apple.com.akadns.net. 59 IN A 17.252.28.16
pop-eur-central-courier.push-apple.com.akadns.net. 59 IN A 17.252.28.10
;AUTHORITY
;ADDITIONAL
id 17387
opcode QUERY
rcode NOERROR
flags QR RD RA
;QUESTION
1-courier.push.apple.com. IN A
;ANSWER
1-courier.push.apple.com. 20218 IN CNAME 1.courier-push-apple.com.akadns.net.
1.courier-push-apple.com.akadns.net. 59 IN CNAME pop-eur-scan-courier.push-apple.com.akadns.net.
pop-eur-scan-courier.push-apple.com.akadns.net. 59 IN A 17.252.108.18
pop-eur-scan-courier.push-apple.com.akadns.net. 59 IN A 17.252.108.16
pop-eur-scan-courier.push-apple.com.akadns.net. 59 IN A 17.252.108.22
pop-eur-scan-courier.push-apple.com.akadns.net. 59 IN A 17.252.108.21
pop-eur-scan-courier.push-apple.com.akadns.net. 59 IN A 17.252.108.30
pop-eur-scan-courier.push-apple.com.akadns.net. 59 IN A 17.252.108.25
pop-eur-scan-courier.push-apple.com.akadns.net. 59 IN A 17.252.108.23
pop-eur-scan-courier.push-apple.com.akadns.net. 59 IN A 17.252.108.8
;AUTHORITY
;ADDITIONAL
id 18897
opcode QUERY
rcode NOERROR
flags QR RD RA
;QUESTION
1-courier.push.apple.com. IN A
;ANSWER
1-courier.push.apple.com. 12757 IN CNAME 1.courier-push-apple.com.akadns.net.
1.courier-push-apple.com.akadns.net. 59 IN CNAME pop-eur-benelux-courier.push-apple.com.akadns.net.
pop-eur-benelux-courier.push-apple.com.akadns.net. 59 IN A 17.252.76.19
pop-eur-benelux-courier.push-apple.com.akadns.net. 59 IN A 17.252.76.13
pop-eur-benelux-courier.push-apple.com.akadns.net. 59 IN A 17.252.76.9
pop-eur-benelux-courier.push-apple.com.akadns.net. 59 IN A 17.252.76.14
pop-eur-benelux-courier.push-apple.com.akadns.net. 59 IN A 17.252.76.8
pop-eur-benelux-courier.push-apple.com.akadns.net. 59 IN A 17.252.76.21
pop-eur-benelux-courier.push-apple.com.akadns.net. 59 IN A 17.252.76.20
pop-eur-benelux-courier.push-apple.com.akadns.net. 59 IN A 17.252.76.30
```
--> Reply depends on geography of probing node


Wrap-up discussion: Now we have found the backend servers for 1 country -- are these all servers? Might some be missing?

### Step 2: Path Measurements using Traceroute (30 min)

Now we can extract a set of IP addresses of backend servers. How to schedule our traceroute measurements? What are the parameters, what is our strategy?

1. Should we measure towards all the IP addresses? Or some? Which? Why?
	* What do you want to learn from the measurements? 
2. What probes should we measure from? All? Some? How to select them? Why do they/do they not represent the APNs user base?
	* Note: In the paper, we just selected 1000 random probes per IP address. A better strategy would have been to only pick probes that were directed towards a certain IP address, but this is significantly more effort. Also, controling for country and source AS might be an idea (but getting 1000 random probes maybe better resembles user population than picking 1 probe per AS)


Task1: Run traceroute measurement towards 1 randomly picked IP address.

After Task1: Which probes? Why? Do they represent user bases? Should we control for AS/CC bias? Should we \eg just use probes that have actually resolved that IP address?

`TODO: Everyone run the measurements for her/his country (only pick DNS replies from that country) OR everyone run global? do range-partitioning on IP addresses to balance load among participants?`

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
