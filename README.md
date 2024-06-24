# UNICEF EAPRO Data Engineer Candidate Take Home Assignment

Dear Candidate,

Below is a modified example of the kinds of real projects you’ll be working on at the Frontier Data Node. To be respectful of your time and help you finish the task in the allotted time, we’ve simplified the scenario and skipped over some important steps (User research, requirements gathering, sprint planning and time estimation). If this were a real project, we’d do all of these first and you’d have an opportunity to propose alternative approaches, ways we can work faster, and different ways of working.

# QUICKSTART:
* A UNICEF Country Office have asked us to estimate the level of internet connectivity for schools in 9 target provinces in Thailand.
* The school locations and their estimated catchment areas are provided in **schools_by_district.geojson**
* Please build a pipeline that retrieves the relevant internet speed and population data
* Please update the provided geojson with the missing values:
    * school_has_connectivity (BOOLEAN)
    * school_download_mbps (FLOAT)
    * school_upload_mbps (FLOAT)
    * catchment_total_children (INT)
    * catchment_average_download_mbps (FLOAT)
    * catchment_average_upload_mbps (FLOAT)
* Datasets you might need:
    * Connectivity: https://registry.opendata.aws/speedtest-global-performance/
    * Population: https://data.humdata.org/dataset/worldpop-age-and-sex-structures-for-thailand
* Python libraries you might find useful (But please use any tools you like, we're tool-agnostic)
    * Geopandas – https://pypi.org/project/geopandas/ 
    * Geowrangler - https://pypi.org/project/geowrangler/
    * PyGeotile – https://pypi.org/project/pyGeoTile/ 
    * Mercantile - https://pypi.org/project/mercantile/

# INSTRUCTIONS - PLEASE:
* Create a branch of this repo and work in that branch.
* Commit code regularly – It’s ok if your code is messy or incomplete, the aim of this take-home assignment is to get a sense of how you work in a real world environment.
* Start with a plan – Write a short text/Markdown doc or start a Jupyter notebook with the steps you’re planning to take and the level of effort for each step (We use T-Shirt sizes – XS, S, M, L, XL but you can use any system that works for you)
* Don’t spend more than 2-3 hours on this – We’re more interested in your problem solving process than in the polish of your code
* If you’re running short on time, please add to your plaintext/markdown document or provide some pseudocode for how you’d approach the remaining tasks. 
    * This won’t count against you, we understand the nature of software development and that sometimes problems are more complex than they first appear
* Use any tool or language that helps you get the task done. If you’re using an external tool that doesn't produce code, please include screenshots or a short plaintext/markdown doc explaining the tool you used and the steps you took.
* We have a preference for Python + Pandas for data manipulation, Airflow for reproducible data pipelines, but we’re open to other ways of working (With a strong preference for open-source tools)
* If you have questions, please ask them by email - [replace with your email] . We’ll try and respond as quickly as possible, but can’t guarantee we’ll be able to respond immediately.
    * Use your best judgement on how to proceed if you don’t hear from us in time.
* The narrative description below tries to give you a sense of how we work as a team within UNICEF. It may have useful information that isn't in the quickstart. Please refer to it if you get stuck


# NARRATIVE VERSION - SCENARIO

FROM: [Chief of Education, Thailand CO]

TO: [Data Specialist, UNICEF EAPRO Frontier Data Node]

CC: [Education Specialist 1, Thailand CO, Education Specialist 2, Thailand CO]

Dear [Data Specialist],

Thank you for the meeting earlier today. We are excited about the potential for frontier data to inform our work.
As discussed, we’re currently working with our partners at the Thai Ministry of Education and [X consultancy] to explore a pilot of our online/e-learning platform, Learning Passport, to 9 Provinces in Thailand. 
However, our Government and implementing partners have raised some concerns as the availability of internet connectivity in those regions. While there are offline options we could explore, we would like to understand the levels of Internet connectivity, both at the schools and the homes of students in each school’s catchment area, so we can provide the best support and advice possible for this program.
Is there any existing frontier data that could help us understand this?

Best Regards,

[Chief of Education, Thailand CO]

---

FROM: [Data Specialist, UNICEF EAPRO Frontier Data Node]

TO: [Chief of Education, Thailand CO]


Dear [Chief],

Thank you for the meeting today, and the opportunity to understand the Thailand Education team’s work. I’ll consult with the team and get back to you tomorrow with some potential solutions.

Best Regards,

[Data Specialist, UNICEF EAPRO Frontier Data Node ]

---

FROM: [Data Specialist, UNICEF EAPRO Frontier Data Node ]

TO: [Data Scientist, UNICEF EAPRO Frontier Data Node]

CC: [Data Visualisation Specialist, UNICEF EAPRO Frontier Data Node, **Data Engineer, UNICEF EAPRO Frontier Data Node(THAT’S YOU!)** ]

HI [Data Scientist] and team,

Any thoughts on how we could answer this?

[Data Specialist, UNICEF EAPRO Frontier Data Node ]

---


FROM: [Data Scientist, UNICEF EAPRO Frontier Data Node]

TO: [Data Specialist, UNICEF EAPRO Frontier Data Node], [Data Visualisation Specialist, UNICEF EAPRO Frontier Data Node, **Data Engineer, UNICEF EAPRO Frontier Data Node(THAT’S YOU!)**]

Hi Data Specialist,

Yep I think we can put together a quick prototype. We have
1.	School locations from OpenStreetMap (8,340 schools in OSM, around 90% coverage vs total schools listed in MOE’s database)
2.	Ookla speedtest data for Thailand (At a glance, coverage looks good – 90% of Bingtiles have at least 4 speed tests in the last year)
3.	High Res population estimates from WorldPop (Age 5-20yrs)

Let me talk to the team and we’ll try and put together a mockup dashboard, summary stats and get the data ready. I’d estimate 2-3 days to put together a mockup and provide some baseline statistics. Does that sound ok?

Thanks,

[Data Scientist]

---

## MS Teams Conversation:


**Data Scientist**: Hi *@team*, did you have a look at this? [LINK to email]

**Data Scientist**: I think we can put a quick prototype together today / tomorrow. Doesn’t need to be production-ready yet, if the Thailand Edu team like it we can build something more robust later.

**Data Scientist**: *@Data Visualisation Specialist* – Can you put together a quick mockup of what a dashboard would look like? (Paper sketch, Figma / Sketch wireframe, HTML / CSS or whatever tool helps you produce it fastest). 
Things they’ll want to know:
1.	Schools

    a.	How many schools in the target province?

    b.	How many have / don’t have internet connectivity?

    c.	Average connection speeds (Upload / Download Mbps) for each school

2.	Children at home

    a.	How many children (5-20yrs old) in each school catchment?
    b.	% of children with any connectivity
    c.	Average connection speeds (Upload / Download Mbps) for the catchment area


**Data Scientist**: **@Data Engineer – THAT’S YOU!**: I’ve created the school catchment area polygons using the province boundaries + travel times. I think we’ll need additional fields for:

* school_has_connectivity (BOOLEAN)

* school_download_mbps (FLOAT)

* school_upload_mbps (FLOAT)

* catchment_total_children (INT)

* catchment_average_download_mbps (FLOAT)

* catchment_average_upload_mbps (FLOAT)

Could you please write a quick pipeline that:
1.	Downloads the Ookla data from Ookla’s S3 bucket - for the most recent 12 months (4 quarters)

    a.	Combines the quarterly data in to a sensible average (Sum of # devices, Mean of upload / download speeds)

2.	Downloads the most recent Worldpop population data from HDX (Preferably using the HDX API)

    a. Sum the total school age population (5-20yrs old) for each school polygon
3.	Merges this data with the existing school polygons geojson
4.	Writes all this to a new geojson file that [Data Visualisation Specialist] can visualise

Please use any tool, language or technology you prefer which will help you get this done fastest – This is strictly for a prototype, if our Thailand CO colleagues show interest, we’ll productionize it later.

Thanks, 

**Data Scientist**


## Reference
These are just suggestions. Approach the task however you think is best, using whatever tools you think are most useful]


Useful data:

* Ookla speedtest data: https://registry.opendata.aws/speedtest-global-performance/

* Worldpop population data: https://data.humdata.org/dataset/worldpop-age-and-sex-structures-for-thailand 

### Useful libraries (Python):
* Geopandas – https://pypi.org/project/geopandas/ 
* Geowrangler - https://pypi.org/project/geowrangler/
* PyGeotile – https://pypi.org/project/pyGeoTile/ 
* Mercantile - https://pypi.org/project/mercantile/ 
