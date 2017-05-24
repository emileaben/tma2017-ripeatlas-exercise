# tma2017-ripeatlas-exercise

## Abstract 

Ripe Atlas is a massively distributed Internet measurement platform that allows for a multitude of pre-defined measurements. In this lab, we will take a research question posed by one of the TMA main conference publications. Step by step, we will discover which features of Ripe Atlas can be used to answer a complex research question, and which subtleties should be taken into consideration when formulating your measurement strategy.
We will walk through scheduling measurements, obtaining, and parsing results, to how to integrate parsed results into your analyses scripts. The specific measurements covered will be distrubted DNS lookups followed by distributed path measurements, representing a typical measurement pipeline in a research paper.

## Details

intro

step1: 50 DNS names -> resolve to IPs with RIPE Atlas probe resolvers
   -> TODO: help with abuf (stephane bortzmeyer tools?)

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
