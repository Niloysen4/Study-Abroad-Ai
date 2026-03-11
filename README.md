# Study-Abroad-Ai
import { useState, useEffect, useRef } from "react";

const COUNTRIES = ["All","USA","UK","Canada","Australia","Germany","Netherlands","Ireland","New Zealand","Japan","Singapore"];
const FIELDS    = ["All","Computer Science","Data Science","Business & MBA","Engineering","Medicine","Law","Arts & Design","Finance","Biotechnology"];
const DEGREES   = ["All","Bachelor's","Master's","PhD","Diploma"];

const UNIVERSITIES = [
  {
    id:1, name:"Massachusetts Institute of Technology", short:"MIT", country:"USA", city:"Cambridge, MA",
    flag:"🇺🇸", color:"#8b0000", rank:1, field:"Computer Science", logo:"M", established:1861, type:"Private Research",
    programs:[
      { name:"MSc Computer Science", degree:"Master's", duration:"2 years", tuition:"$59,750/yr", currency:"USD", intake:["September"], spots:120,
        requirements:{ gpa:"3.7+", pte:"79+", ielts:"7.0+", gre:"Required", workExp:"Recommended" },
        courses:["Machine Learning","Algorithms","Distributed Systems","AI Ethics","Deep Learning","Computer Vision"],
        description:"World-leading CS program with access to MIT CSAIL, the largest AI research laboratory in the US.",
        deadline:"Dec 15", scholarships:true, onlineOption:false },
      { name:"BSc Computer Science & Engineering", degree:"Bachelor's", duration:"4 years", tuition:"$57,986/yr", currency:"USD", intake:["September"], spots:200,
        requirements:{ gpa:"3.9+", pte:"80+", ielts:"7.5+", sat:"1560+", act:"35+" },
        courses:["Data Structures","Operating Systems","Web Technology","Robotics","Quantum Computing","Software Engineering"],
        description:"The most rigorous CS undergraduate in the world. Gateway to top tech careers and research.",
        deadline:"Jan 1", scholarships:true, onlineOption:false },
    ],
    campusLife:"150+ student clubs, world-class labs, Kendall Square tech hub nearby, strong startup culture.", livingCost:"$2,200–$3,000/month", accommodation:"On-campus guaranteed year 1",
    visaInfo:"F-1 Student Visa. OPT available post-graduation (up to 3 years for STEM). SEVIS fee required.", workRights:"20hrs/week on-campus, full-time during official breaks", website:"mit.edu", match:96
  },
  {
    id:2, name:"University of Oxford", short:"Oxford", country:"UK", city:"Oxford, England",
    flag:"🇬🇧", color:"#002147", rank:3, field:"Business & MBA", logo:"O", established:1096, type:"Public Research",
    programs:[
      { name:"MSc Financial Economics", degree:"Master's", duration:"1 year", tuition:"£35,860/yr", currency:"GBP", intake:["October"], spots:80,
        requirements:{ gpa:"3.6+", pte:"76+", ielts:"7.5+", gmat:"680+" },
        courses:["Macroeconomics","Corporate Finance","Econometrics","Behavioural Finance","Global Markets","Investment Theory"],
        description:"One of the world's most prestigious postgraduate economics programs. Saïd Business School alumni in top global firms.",
        deadline:"Jan 20", scholarships:true, onlineOption:false },
      { name:"DPhil Computer Science", degree:"PhD", duration:"3–4 years", tuition:"£28,050/yr", currency:"GBP", intake:["October","January"], spots:40,
        requirements:{ gpa:"3.8+", pte:"80+", ielts:"7.5+", publications:"Preferred" },
        courses:["Research Methods","Advanced Algorithms","Human-Computer Interaction","Robotics","Formal Verification"],
        description:"Research doctorate at Oxford's Department of Computer Science. Access to world-class supervisors.",
        deadline:"Rolling", scholarships:true, onlineOption:false },
    ],
    campusLife:"38 independent colleges, historic Bodleian Library, Oxford Union debates, rowing & punting on the Thames.", livingCost:"£1,400–£1,900/month", accommodation:"College accommodation for most students",
    visaInfo:"UK Student Visa (formerly Tier 4). CAS number issued by university. Biometrics and NHS surcharge required.", workRights:"20hrs/week term-time, unlimited during vacations", website:"ox.ac.uk", match:91
  },
  {
    id:3, name:"University of Toronto", short:"U of T", country:"Canada", city:"Toronto, ON",
    flag:"🇨🇦", color:"#00204e", rank:18, field:"Data Science", logo:"T", established:1827, type:"Public Research",
    programs:[
      { name:"MScAC – Applied Computing", degree:"Master's", duration:"16 months", tuition:"C$58,000 total", currency:"CAD", intake:["September"], spots:80,
        requirements:{ gpa:"3.5+", pte:"72+", ielts:"6.5+", gre:"Optional" },
        courses:["Deep Learning","Big Data","Cloud Computing","Software Engineering","NLP","Computer Vision","Capstone Project"],
        description:"Industry-focused master's with mandatory paid internship in Toronto's booming tech scene. 95% employment rate.",
        deadline:"Feb 1", scholarships:true, onlineOption:false },
      { name:"MEng Electrical & Computer Engineering", degree:"Master's", duration:"1 year", tuition:"C$35,440/yr", currency:"CAD", intake:["September","January"], spots:150,
        requirements:{ gpa:"3.3+", pte:"70+", ielts:"6.5+" },
        courses:["Signal Processing","Embedded Systems","VLSI Design","Power Electronics","Control Systems"],
        description:"Coursework-based master's ideal for fast-track to industry. Engineering alumni network of 100,000+.",
        deadline:"Mar 15", scholarships:false, onlineOption:true },
    ],
    campusLife:"1,000+ clubs, St. George campus in downtown Toronto, diverse city of 3M, proximity to Silicon Valley North.", livingCost:"C$2,000–C$2,800/month", accommodation:"Graduate residences available",
    visaInfo:"Canadian Study Permit. Post-Graduation Work Permit (PGWP) up to 3 years. Apply 6 weeks before start.", workRights:"20hrs/week during study, full-time during scheduled breaks", website:"utoronto.ca", match:94
  },
  {
    id:4, name:"TU Munich (TUM)", short:"TUM", country:"Germany", city:"Munich, Bavaria",
    flag:"🇩🇪", color:"#003366", rank:49, field:"Engineering", logo:"T", established:1868, type:"Public Technical University",
    programs:[
      { name:"MSc Robotics, Cognition, Intelligence", degree:"Master's", duration:"2 years", tuition:"€174/semester", currency:"EUR", intake:["October","April"], spots:100,
        requirements:{ gpa:"3.3+", pte:"65+", ielts:"6.5+", gre:"Optional" },
        courses:["Robot Motion Planning","Cognitive Systems","Computer Vision","Reinforcement Learning","Sensor Fusion","ROS2"],
        description:"Europe's #1 robotics program. Near-free tuition, world-class facilities, BMW & Siemens partnerships.",
        deadline:"Jan 31 / Jul 31", scholarships:true, onlineOption:false },
      { name:"MSc Informatics", degree:"Master's", duration:"2 years", tuition:"€174/semester", currency:"EUR", intake:["October"], spots:120,
        requirements:{ gpa:"3.3+", pte:"65+", ielts:"6.5+" },
        courses:["Algorithms & Complexity","Database Systems","Software Engineering","Network Security","HCI"],
        description:"Top-ranked CS master's at minimal cost. Munich's tech ecosystem includes Google, Apple, Microsoft offices.",
        deadline:"Jan 15", scholarships:true, onlineOption:false },
    ],
    campusLife:"Munich culture, Oktoberfest, Alps day trips, strong startup ecosystem, TUM Venture Labs.", livingCost:"€1,200–€1,600/month", accommodation:"Studentenwerk halls ~€400/month",
    visaInfo:"German National Visa (Type D). Blocked account of €11,208/yr required. Enrolment proof needed.", workRights:"120 full days or 240 half-days per year", website:"tum.de", match:89
  },
  {
    id:5, name:"University of Melbourne", short:"UniMelb", country:"Australia", city:"Melbourne, VIC",
    flag:"🇦🇺", color:"#003087", rank:33, field:"Data Science", logo:"M", established:1853, type:"Public Research",
    programs:[
      { name:"Master of Data Science", degree:"Master's", duration:"2 years", tuition:"A$47,392/yr", currency:"AUD", intake:["February","July"], spots:200,
        requirements:{ gpa:"3.0+", pte:"72+", ielts:"6.5+", workExp:"Preferred" },
        courses:["Statistical Learning","Data Visualisation","Cloud Computing","Ethics in AI","Predictive Analytics","Time Series Analysis"],
        description:"Australia's #1 data science program. Industry partnerships with ANZ, NAB, IBM, Seek.",
        deadline:"Oct 31 / Apr 30", scholarships:true, onlineOption:false },
      { name:"Master of Information Technology", degree:"Master's", duration:"1.5–2 years", tuition:"A$45,000/yr", currency:"AUD", intake:["February","July"], spots:250,
        requirements:{ gpa:"2.8+", pte:"70+", ielts:"6.5+" },
        courses:["Software Architecture","Cybersecurity","Agile Development","Mobile Apps","IT Project Management"],
        description:"Practical IT master's with Melbourne Connect industry hub access. Strong graduate outcomes.",
        deadline:"Rolling", scholarships:false, onlineOption:true },
    ],
    campusLife:"Parkville campus, Melbourne's café culture, AFL footy, multicultural city ranked #1 liveable globally.", livingCost:"A$2,500–A$3,200/month", accommodation:"On-campus colleges and off-campus options",
    visaInfo:"Student Visa (Subclass 500). OSHC health insurance mandatory. Must maintain 80% attendance.", workRights:"48hrs/fortnight during study, unlimited during holidays", website:"unimelb.edu.au", match:87
  },
  {
    id:6, name:"ETH Zürich", short:"ETH", country:"Germany", city:"Zürich, Switzerland",
    flag:"🇨🇭", color:"#1d5fa6", rank:7, field:"Engineering", logo:"E", established:1855, type:"Public Technical University",
    programs:[
      { name:"MSc Electrical Engineering & IT", degree:"Master's", duration:"2 years", tuition:"CHF 730/semester", currency:"CHF", intake:["September"], spots:80,
        requirements:{ gpa:"3.5+", pte:"72+", ielts:"7.0+" },
        courses:["Power Systems","Quantum Engineering","Photonics","Communication Networks","VLSI Design"],
        description:"Top-10 globally for engineering. Einstein's alma mater. Strong pharma & finance industry ties.",
        deadline:"Dec 15", scholarships:true, onlineOption:false },
    ],
    campusLife:"Zürich's safe lakeside city, SBB rail, skiing in the Alps, world-renowned food & culture.", livingCost:"CHF 2,000–2,800/month", accommodation:"Student housing via WOKO",
    visaInfo:"Switzerland Residence Permit (L/B permit). EU/non-EU rules differ. Apply 3 months before arrival.", workRights:"15hrs/week permitted", website:"ethz.ch", match:85
  },
  {
    id:7, name:"National University of Singapore", short:"NUS", country:"Singapore", city:"Singapore",
    flag:"🇸🇬", color:"#003D7C", rank:8, field:"Computer Science", logo:"N", established:1905, type:"Public Research",
    programs:[
      { name:"MSc Computer Science", degree:"Master's", duration:"1 year", tuition:"S$35,200 total", currency:"SGD", intake:["August","January"], spots:160,
        requirements:{ gpa:"3.2+", pte:"67+", ielts:"6.0+" },
        courses:["Parallel Computing","Blockchain","AI & Society","Multimedia Systems","Programming Languages","Distributed Computing"],
        description:"Asia's top CS program — gateway to Singapore's thriving tech ecosystem. Fast-track 1-year option.",
        deadline:"Feb 15 / Sep 15", scholarships:true, onlineOption:false },
    ],
    campusLife:"Tropical campus, iconic hawker centre food, regional travel hub, vibrant finance & tech sector.", livingCost:"S$2,000–S$2,800/month", accommodation:"On-campus hostels available",
    visaInfo:"Singapore Student's Pass issued by ICA. Apply via Student's Pass Online Application (SOLAR).", workRights:"16hrs/week, full-time vacations", website:"nus.edu.sg", match:83
  },
  {
    id:8, name:"Delft University of Technology", short:"TU Delft", country:"Netherlands", city:"Delft",
    flag:"🇳🇱", color:"#00A6D6", rank:57, field:"Engineering", logo:"D", established:1842, type:"Public Technical University",
    programs:[
      { name:"MSc Aerospace Engineering", degree:"Master's", duration:"2 years", tuition:"€18,750/yr", currency:"EUR", intake:["September"], spots:120,
        requirements:{ gpa:"3.2+", pte:"67+", ielts:"6.5+" },
        courses:["Aerodynamics","Spacecraft Design","Structural Analysis","Propulsion","Flight Dynamics","Systems Integration"],
        description:"Europe's premier aerospace program. Airbus, ESA, Fokker partnerships. Wind tunnel facilities.",
        deadline:"Apr 1", scholarships:true, onlineOption:false },
      { name:"MSc Computer Science", degree:"Master's", duration:"2 years", tuition:"€17,500/yr", currency:"EUR", intake:["September"], spots:100,
        requirements:{ gpa:"3.0+", pte:"65+", ielts:"6.0+" },
        courses:["Software Technology","Data Management","Intelligent Systems","Embedded Software","Network Security"],
        description:"Strong research focus in the Dutch tech corridor — 30 min to Amsterdam.", deadline:"May 1", scholarships:false, onlineOption:false },
    ],
    campusLife:"Bike-friendly Delft, 30 min to Amsterdam, innovative Green Village campus, Dutch openness.", livingCost:"€1,100–€1,600/month", accommodation:"SSH student housing",
    visaInfo:"MVV Entry Visa then Residence Permit via IND. Insurance required. University assists with applications.", workRights:"16hrs/week, full-time June–August", website:"tudelft.nl", match:82
  },
  {
    id:9, name:"University College London", short:"UCL", country:"UK", city:"London, England",
    flag:"🇬🇧", color:"#500778", rank:9, field:"Finance", logo:"U", established:1826, type:"Public Research",
    programs:[
      { name:"MSc Finance", degree:"Master's", duration:"1 year", tuition:"£36,000/yr", currency:"GBP", intake:["September"], spots:100,
        requirements:{ gpa:"3.4+", pte:"76+", ielts:"7.0+", gmat:"650+" },
        courses:["Asset Pricing","Corporate Finance","Derivatives","Risk Management","FinTech","Quantitative Methods"],
        description:"London's premier finance gateway. Graduate recruitment by Goldman, JPMorgan, HSBC, BlackRock.",
        deadline:"Mar 31", scholarships:true, onlineOption:false },
      { name:"MSc Machine Learning", degree:"Master's", duration:"1 year", tuition:"£33,200/yr", currency:"GBP", intake:["September"], spots:60,
        requirements:{ gpa:"3.5+", pte:"76+", ielts:"7.0+", gre:"Recommended" },
        courses:["Probabilistic Inference","Reinforcement Learning","Kernel Methods","Bayesian Deep Learning","NLP"],
        description:"Cutting-edge ML program with Gatsby Unit proximity. World-class faculty in Bloomsbury.",
        deadline:"Apr 15", scholarships:true, onlineOption:false },
    ],
    campusLife:"Central London campus, 200+ societies, Bloomsbury culture, access to global finance and tech hubs.", livingCost:"£1,800–£2,600/month", accommodation:"UCL halls and private options",
    visaInfo:"UK Student Visa. CAS number from university. English language proof & financial evidence required.", workRights:"20hrs/week term-time, unlimited vacations", website:"ucl.ac.uk", match:88
  },
  {
    id:10, name:"McGill University", short:"McGill", country:"Canada", city:"Montréal, QC",
    flag:"🇨🇦", color:"#ed1b2f", rank:44, field:"Medicine", logo:"M", established:1821, type:"Public Research",
    programs:[
      { name:"MD – Doctor of Medicine", degree:"PhD", duration:"4 years", tuition:"C$24,000/yr", currency:"CAD", intake:["August"], spots:180,
        requirements:{ gpa:"3.8+", pte:"80+", ielts:"7.5+", mcat:"515+" },
        courses:["Anatomy","Physiology","Pharmacology","Clinical Skills","Pathology","Surgery Clerkship","Community Medicine"],
        description:"Top-5 Canadian medical school. Affiliated with 6 teaching hospitals. PBL curriculum.",
        deadline:"Nov 1", scholarships:true, onlineOption:false },
      { name:"MSc Artificial Intelligence", degree:"Master's", duration:"2 years", tuition:"C$22,000/yr", currency:"CAD", intake:["September"], spots:60,
        requirements:{ gpa:"3.5+", pte:"72+", ielts:"6.5+" },
        courses:["Probabilistic Learning","Cognitive Modelling","AI Ethics","Computer Vision","Multi-Agent Systems"],
        description:"Montreal AI hub — MILA is next door, founded by Turing Award winner Yoshua Bengio.",
        deadline:"Jan 15", scholarships:true, onlineOption:false },
    ],
    campusLife:"Bilingual French-English Montreal, vibrant student scene, cheapest major Canadian city, great nightlife.", livingCost:"C$1,600–C$2,200/month", accommodation:"Residences & affordable off-campus",
    visaInfo:"Canadian Study Permit. PGWP up to 3 years post-graduation. Biometrics required.", workRights:"20hrs/week", website:"mcgill.ca", match:80
  },
  {
    id:11, name:"University of Edinburgh", short:"Edinburgh", country:"UK", city:"Edinburgh, Scotland",
    flag:"🏴󠁧󠁢󠁳󠁣󠁴󠁿", color:"#041E42", rank:22, field:"Business & MBA", logo:"E", established:1583, type:"Public Research",
    programs:[
      { name:"Full-Time MBA", degree:"Master's", duration:"1 year", tuition:"£40,500/yr", currency:"GBP", intake:["September"], spots:80,
        requirements:{ gpa:"3.2+", pte:"72+", ielts:"7.0+", gmat:"600+", workExp:"3+ years" },
        courses:["Strategic Management","Leadership","Marketing","Global Operations","Entrepreneurship","Business Analytics","Innovation"],
        description:"Triple-accredited MBA (AACSB, EQUIS, AMBA). UNESCO World Heritage campus. Strong alumni network.",
        deadline:"Jun 30", scholarships:true, onlineOption:false },
    ],
    campusLife:"Historic Old Town, Edinburgh Festival & Fringe, Highland adventures, safe walkable city.", livingCost:"£1,200–£1,800/month", accommodation:"Postgrad accommodation blocks",
    visaInfo:"UK Student Visa. CAS issued by university. Healthcare surcharge included.", workRights:"20hrs/week term-time", website:"ed.ac.uk", match:86
  },
  {
    id:12, name:"Trinity College Dublin", short:"TCD", country:"Ireland", city:"Dublin",
    flag:"🇮🇪", color:"#003399", rank:134, field:"Computer Science", logo:"T", established:1592, type:"Public Research",
    programs:[
      { name:"MSc Computer Science (Data Science)", degree:"Master's", duration:"1 year", tuition:"€17,930/yr", currency:"EUR", intake:["September"], spots:50,
        requirements:{ gpa:"3.0+", pte:"63+", ielts:"6.5+" },
        courses:["Machine Learning","Data Engineering","Statistical Inference","Big Data","Visualisation","Applied AI"],
        description:"Ireland's oldest university. Dublin is EU home to Google, Meta, Microsoft, LinkedIn, Stripe.",
        deadline:"Jun 1", scholarships:true, onlineOption:false },
    ],
    campusLife:"City-centre campus, Temple Bar cultural quarter, vibrant pub culture, affordable vs London/Paris.", livingCost:"€1,500–€2,200/month", accommodation:"Trinity accommodation and private rentals",
    visaInfo:"Irish Study Visa for non-EU students. Registration with GNIB/IRP within 90 days. Stamp 2 visa.", workRights:"20hrs/week term-time, 40hrs/week during holidays", website:"tcd.ie", match:78
  },

  // ── NEW ZEALAND UNIVERSITIES ──
  {
    id:13, name:"University of Auckland — Te Whare Wānanga o Tāmaki Makaurau", short:"UoA", country:"New Zealand", city:"Auckland",
    flag:"🇳🇿", color:"#003B6F", rank:68, field:"Computer Science", logo:"A", established:1883, type:"Public Research",
    nz:true,
    programs:[
      { name:"Master of Information Technology (MIT)", degree:"Master's", duration:"1.5–2 years", tuition:"NZ$43,000/yr", currency:"NZD", intake:["February","July"], spots:120,
        requirements:{ gpa:"3.2+", pte:"65+", ielts:"6.5+", workExp:"Recommended" },
        courses:["Advanced Databases","Cloud & IoT Architecture","Cybersecurity","Machine Learning","Software Engineering","Project Management","Data Governance"],
        description:"New Zealand's #1 university. 2025 updated curriculum includes Cloud-Native Development and AI Ethics modules. Strong tech industry ties with Datacom, Spark, Fisher & Paykel.",
        deadline:"Nov 30 / May 31", scholarships:true, onlineOption:false,
        recentUpdate:"2025: New AI & Data Governance specialisation added. Partnership with AWS Academ