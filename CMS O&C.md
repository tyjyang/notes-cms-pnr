The CMS detector collects relics of energetic particle collisions
to inform us on how the fundamental particles interact with one another.  
The initial data we collect are in forms of electronic signals
in the millions of channels in the detector.  
To convert that to meaningful physics information,
we need to manipulate on the raw detector outputs.
For example, we want to **reconstruct** the energy and momentum of particles.  
Other than that, we also want to cut out irrelevant information in the dataset,
so that they become small enough for easy analysis.  
Part of the data processing are done **online** as they are collected,
but due to constraints in real-time processing power,
a lot of data processing is done **offline**,
after the data is **transferred** and **stored** at CMS computing sites all over the world.

The "R" in PNR thus stands for **Data Reprocessing**.

Having the data alone is not sufficient.  
Due to a plethora of complicated underlining physics processes,
one can only realistically rely on computer simulations (with the Monte Carlo method)
to tell if the data shows any signal process of one's interest.  
The Monte Carlo samples can be computationally-heavy,
especially if one wants to have matching statistics (comparable number of events) with the data.  
Therefore, the CMS Offline and Computing team is also responsible for
the production of the Monte Carlo samples using all computing resources within the reach of CMS.

The "P" in PNR stand for **MC Production**.

