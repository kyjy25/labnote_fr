# General Task Breakdown

### Automation Components Preparation

Identify how many components are missing

* Servo motors
* Pi boards
* Wires \&Leads
* TEC
* DAQ
* Thermistors
* Cables
* Adapters
* Powers, Power Strips
* ...

Identify their corresponding properties and expected performances we need

For servos, analyze how many motional DOF are there in the cavity to determine the amount of servos we need.

Purchase selection, taking into account their delivery time. Place orders.

### Components Test & Benchmark

Servo benchmark, including hysteresis determination (maybe re-use the test done before if servos are same?)

Soldering work, including Pi boards (Maybe send to electrical workshop)

### Artiq Integration

Check all of their divers, like&#x20;

* PZT driver
* Photodiode readout (via DAQ?)

Practically connect them into Artiq network, test their performance and compatibility

Make Artiq system actually take over the automation work.

### 3D Print

Identify how many different 3D printings we need.

Measure their geometry, make 3D models and print.

Assembly test and revision printing.

### Assembly

Cleaning work

Nitrogen pumping to reduce contamination induced by photochemical reaction

### Characterization of Automation Feedback Signal Stability

Identify possible noise/drift sources, including&#x20;

* **Temperature noise** (MCD, TH10K sensors)
* **PZT drift, cavity length drift**
* **Input laser power fluctuations**

**Rank them by their dominancy**

* Measure sensitivity of SHG output to alignment, temperature, and cavity length.

A dataset demonstrating:

* How unstable the system is without automation
* Which parameters dominate SHG efficiency

### Optical Alignment

### Software \&Program

FInd the suitable algorithm, maybe re-use the one in diamond setup

Determine the optimal parameters

Automation program test

Test reliability and quality of automated SHG&#x20;



